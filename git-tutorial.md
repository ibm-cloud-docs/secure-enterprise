---

copyright:

  years: 2024, 2026

lastupdated: "2026-01-13"

keywords: Git, Git integration, Connect to Git, Github, Gitlab, GitHub enterprise, pipelines, toolchains, workflow

subcollection: secure-enterprise

content-type: tutorial
account-plan: lite
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Automating projects actions in your Git repository
{: #tutorial-git-integration}
{: toc-content-type="tutorial"} 
{: toc-completion-time="30m"} 

In this tutorial, you learn how to set up a pipeline to trigger an update in your project when configuration changes are merged to the main branch in your repository. By completing this tutorial, you learn how to automate common tasks in a project, such as validating a configuration, by using the pipelines and toolchains of your choosing.  
{: shortdesc}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

This tutorial focuses on a simple use case of updating a configuration in a project when changes are merged to the main branch in your repository, as illustrated in the following image: 

![The image shows an update that goes from the project to a side branch in the repository. Then, an arrow indicates that the side branch is merged into the main branch, which triggers the CD pipeline. The CD pipeline includes a trigger to update the configurations in your project based on the updates in your main branch.](images/git-integrations-tutorial.svg "Automatically updating your project after changes are merged into your main branch"){: caption="Automatically updating your project after changes are merged into your main branch" caption-side="bottom"}

Since pipelines and toolchains are customizable, the principles in this tutorial can help you automate other common actions within a project, such as validating and deploying configuration changes after they are merged to the main branch in your repository. This tutorial uses GitHub actions and workflows to automate a pipeline between the repository and the project. As you complete the tutorial, adapt each step to match your repository's CI and CD pipelines and processes. 

You can validate and deploy draft configurations only after they are merged to the branch in your repository that manages your CD pipelines. You must also sync the updates from your repository into your project by updating configurations before you can validate and deploy those configurations. You can automate this update by using the [`project.config.update`](/apidocs/projects#update-config) API method (as described in this tutorial) or by using the [`ibmcloud project config-update`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-update-command) CLI command.
{: important}

## Before you begin
{: #git-integration-byb}

1. Make sure you have the Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
1. Complete the steps to [connect your project to a Git repository](/docs/secure-enterprise?topic=secure-enterprise-connect-to-git&interface=ui). For the purposes of this tutorial, connect an empty project to a GitHub repository. 

## Adding secrets and variables to GitHub
{: #github-action-validate}
{: step}

Create secrets and variables in the GitHub repository that you connected to your project. These secrets and variables are used in the GitHub workflow. Add the following secrets and variables:  

1. To authenticate with your project, you must include an {{site.data.keyword.cloud_notm}} API key in your GitHub workflow. To keep the API key secure, complete the steps to [create a secret for a repository](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets#creating-secrets-for-a-repository){: external} to save the API key as a secret in GitHub. For the purposes of this tutorial, name the secret `IBM_CLOUD_API_KEY`. 
1. Next, complete the steps to [create configuration variables](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-variables#creating-configuration-variables-for-a-repository){: external} for your GitHub repository. Save the following variables: 

   | Variable name  | Value | Description |
   |------------------|-------|-------|
   | `CONFIG_FOLDER_PATH` | `configs` | The path to the repository folder that is connected to your project. This folder contains the configuration files from your project. |
   | `IAM_URL` | `https://iam.cloud.ibm.com` | The URL to {{site.data.keyword.iamshort}}. |
   | `PROJECTS_API_BASE_URL` | `https://projects.api.cloud.ibm.com` | The URL to the projects API. |
   {: caption="List of variables to save to your GitHub repository" caption-side="bottom"}

## Creating a workflow in GitHub 
{: #github-workflow-create}
{: step}

Complete the steps to [write a workflow](https://docs.github.com/en/enterprise-cloud@latest/actions/get-started/quickstart){: external} in the GitHub repository that you connected to your project. 

Get started with an [example workflow file](#example-workflow) that you can modify as needed in GitHub. 
{: tip}

You can customize the workflow with any number of jobs that you require. However, the following code needs to be included to successfully update configurations in your project when changes are merged from a side branch into the main branch of your repository: 

1. Add `types: [closed]` to the `on` section of the workflow to trigger the workflow when a pull request to the main branch is closed: 
   
   ```sh
      # Controls when the workflow will run
      on:
        # Triggers the workflow on push or pull request events but only for the "main" branch
        # push:
        #   branches: [ "main" ]
        pull_request:
          branches: [ "main" ]
          types: [closed]
   ```
   {: codeblock}

1. Add an `if` statement that triggers this workflow when changes are merged to the main branch:
   
   ```sh
   jobs:
     update-config:
       if: github.event.pull_request.merged == true
       runs-on: ubuntu-latest
   ```
   {: codeblock}

1. Include the following code in the workflow so your repository can connect to your project, where `IBM_CLOUD_API_KEY` is the name of the secret you added to GitHub: 

   ```sh
     IAM_TOKEN=$(curl -X POST "https://iam.cloud.ibm.com/identity/token" \
           -H "Content-Type: application/x-www-form-urlencoded" \
            -H "Accept: application/json" \
            -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=${{ secrets.IBM_CLOUD_API_KEY }}" | jq -r .access_token)
   ```
   {: codeblock}

1. Include the following code to identify which configurations were edited: 

   ```sh
             # get files changed in the PR
             changed_files=$(git diff --name-only HEAD^ HEAD)
             echo "Changed files: $changed_files"

             for file in $changed_files; do
               # find config files that were changed
               if [[ "${file}" == ${{ vars.CONFIG_FOLDER_PATH }}/* ]] && [ -s "${file}" ]; then
                 echo "Config file updated: ${file}"
                            
                 # extract data from config files
                 PROJECT_ID=$(jq -r '.project_id' $file)
                 CONFIG_ID=$(jq -r '.config_id' $file)
                 DEF=$(jq '.definition' $file)

                 echo "Project ID: ${PROJECT_ID}"
                 echo "Config ID: ${CONFIG_ID}"
   ```
   {: codeblock}

1. Include the following code to update the edited configurations in your project: 

   ```sh
                 # update config definition
                 RESPONSE=$(curl -X PATCH "${BASE_URL}/v1/projects/${PROJECT_ID}/configs/${CONFIG_ID}" \
                 --header "Authorization: Bearer ${IAM_TOKEN}" \
                 --header "Accept: application/json" \
                 --header "Content-Type: application/json" \
                 --data "{ \"definition\": ${DEF} }")
   ```
   {: codeblock}

### Example workflow file
{: #example-workflow}

The following code snippet can be used as a template for your workflow file: 

```sh
name: Projects Git Integration Workflow

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  pull_request:
    branches: [ "main" ]
    types: [closed]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  update-configs:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Update Project Configs
        run: |
          IAM_TOKEN=$(curl -X POST "${{ vars.IAM_URL }}/identity/token" \
          -H "Content-Type: application/x-www-form-urlencoded" \
          -H "Accept: application/json" \
          -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=${{ secrets.IBM_CLOUD_API_KEY }}" | jq -r .access_token)

          BASE_URL=${{ vars.PROJECTS_API_BASE_URL }}

          # get files changed in the PR
          changed_files=$(git diff --name-only HEAD^ HEAD)
          echo "Changed files: $changed_files"

          for file in $changed_files; do
            # find config files that were changed
            if [[ "${file}" == ${{ vars.CONFIG_FOLDER_PATH }}/* ]] && [ -s "${file}" ]; then
              echo "Config file updated: ${file}"
                            
              # extract data from config files
              PROJECT_ID=$(jq -r '.project_id' $file)
              CONFIG_ID=$(jq -r '.config_id' $file)
              DEF=$(jq '.definition' $file)

              echo "Project ID: ${PROJECT_ID}"
              echo "Config ID: ${CONFIG_ID}"

              # update config definition
              RESPONSE=$(curl -X PATCH "${BASE_URL}/v1/projects/${PROJECT_ID}/configs/${CONFIG_ID}" \
              --header "Authorization: Bearer ${IAM_TOKEN}" \
              --header "Accept: application/json" \
              --header "Content-Type: application/json" \
              --data "{ \"definition\": ${DEF} }")

              echo $RESPONSE
              ERR_CODE=$(echo $RESPONSE | jq '.code')
              if [ "${ERR_CODE}" != "null" ]; then
                exit 1
              fi
            else
              echo "Not a project configuration file: ${file}"
            fi
          done
```
{: codeblock}

## Testing the workflow 
{: #git-next-steps}
{: step}

Now that your workflow is created in GitHub, make sure that the workflow runs successfully by adding a configuration to the project. Complete the following steps: 

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)** and select the project that is connected to your GitHub repository. 
1. Click **Create** to add a configuration to your project. Make sure to select a side branch where your configuration will be saved.
1. Edit the configuration. For example, add an authentication method in the **Configure** panel. 
1. Select the side branch where you want to commit your changes. 
1. Click **Commit**. 
1. Go to your GitHub repository, and open a pull request to merge the side branch into the main branch of your repository. 
1. Verify that your workflow runs when the side branch is merged into the main one. You can verify in the GitHub repository and the project: 

   1. In the GitHub repository, go to the **Actions** tab and select the workflow. Verify that the update ran successfully. 
   1. In your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg) > **Edit** for the configuration you added and switch to the main branch. Verify that your update is applied. For example, the authentication method that you added earlier is included in the **Configure** panel. 
