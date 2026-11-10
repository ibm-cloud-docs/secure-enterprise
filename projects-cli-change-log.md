---

copyright:
  years:  2023, 2026
lastupdated: "2026-11-09"

keywords: change log for Project, updates to Project CLI

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Project CLI change log
{: #cli-change-log}

In this change log, you can learn about the latest changes, improvements, and updates for the {{site.data.keyword.cloud_notm}} Project CLI.
{: shortdesc}

## Version 0.0.37
{: #cli-0036}

Version 0.0.37 of the CLI was released on 3 April 2024.

In this release, the following changes impact the pagination of listing commands. For more information, see the [pagination](/apidocs/projects#get-config-version-response) section of the Projects API docs. The in-line option `--start` was renamed to `--token` for all listing commands. For example, the `project list` command now uses `--token` instead of `--start` as an option.

Listing commands return 10 records by default if the page size is not specified by the `--limit` option. Alternatively, the `--all-pages` option returns every record available.

Added experimental commands to support [stacking deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=cli).

## Version 0.0.25
{: #cli-0024}

Version 0.0.25 of the CLI was released on 6 November 2023.

In this release, the `create` and `update` JSON properties match the `get` JSON properties with separation between the server-controlled attributes and the user-supplied attributes. The commands are normalized so that they follow a regular pattern. Terminology was also aligned to use `validate`, `deploy`, and `undeploy`.

Updated the following commands:
- All commands that impact configurations now begin with `config` instead of `project`.
- The **`project create`**, **`project update`**, **`config-create`**, and **`config-update`** commands now use the `definition` construct to replace individual parameters like `name` and `description`.
- The **`project config-install`** command is replaced by the **`config-deploy`** command.
- The **`project config-uninstall`** command is replaced by the **`config-undeploy`** command.
- The **`project list-config-drafts`** command is replaced by the **`config-versions`** command.
- The **`project config-draft-get`** command is replaced by the **`config-version-get`** command.

Added the following commands:
- The **`config-version-delete`** command deletes a version of a configuration.
- The **`environment-create`** command creates an environment.
- The **`environment-update`** command edits an environment.
- The **`environment-delete`** command deletes an environment.
- The **`environment`** command returns an environment.
- The **`environments`** command returns all environments.
