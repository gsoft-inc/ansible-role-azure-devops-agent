# Azure DevOps Agent

An Ansible role that installs and configures a Linux machine to be used as an [Azure DevOps build or deployment agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=vsts).

See [this blog post](https://medium.com/gsoft-tech/easily-configuring-an-azure-devops-agent-with-ansible-fb9cb0f98b73) for more detail.

## Requirements

[See prerequisites](https://github.com/Microsoft/azure-pipelines-agent/blob/master/docs/start/envlinux.md)

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    az_devops_accountname: null
    az_devops_accesstoken: null
    az_devops_project_name: null
    az_devops_agent_version: 2.165.1
    az_devops_agent_user: "az_devops_agent"
    az_devops_agent_name: "{{ ansible_hostname }}"
    az_devops_server_url: "https://dev.azure.com/{{ az_devops_accountname }}"
    az_devops_agent_folder: "/home/{{ az_devops_agent_user }}/agent/"
    az_devops_work_folder: "/home/{{ az_devops_agent_user }}/agent/_work"
    az_devops_agent_pool_name: "Default"
    az_devops_agent_role: "build"
    az_devops_deployment_group_tags: null
    az_devops_deployment_group_name: "Default"
    az_devops_environment_name: null
    az_devops_agent_replace_existing: false
    az_devops_reconfigure_agent: false
    az_devops_agent_user_capabilties:
      user_capabilty_key: user_capability_value

- **az_devops_accountname**

  The name of your Azure DevOps account, i.e. ttps://dev.azure.com/YOUR_ACCOUNT_NAME

- **az_devops_accesstoken**

  The Personal Access Token (PAT) used to authenticate to your account. [See here for details on how to generate this value](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=vsts#authenticate-with-a-personal-access-token-pat).

  _Note: Think about using Ansible Vault to secure this value._

- **az_devops_project_name**

  The name of the Azure DevOps project in which to register the agent (only used for deployment groups).

- **az_devops_agent_version**

  Version of the installed agent package. Should be periodically updated to the latest version (see [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=vsts#download-and-configure-the-agent))

- **az_devops_agent_user**

  Name of the user used to run and configure the service.

- **az_devops_agent_name**

  Name of the agent shown in Azure DevOps (defaults to the name of the host.).

- **az_devops_server_url**

  Url for your Azure DevOps account.

- **az_devops_agent_folder**

  Folder location for all the agent specific files (note: important that the service user needs execution permissions on all the files in this folder).

- **az_devops_work_folder**

  Folder location for all the work specific files (i.e. pulled source code and build results).

- **az_devops_agent_pool_name**

  Pool name in which the Azure DevOps agent is added.

- **az_devops_agent_role**

  Use either `build`, `deployment` or `resource`. Build role allows the use of the agent as a build server in pipeline build or releases. Deployment role allows the use of the agent in a deployment group. Resource role allows the use of the agent as a virtual machine resource that can be targeted by deployments from a pipeline and belongs to an environment.

- **az_devops_deployment_group_tags**

  Use in conjuction with the `deployment` agent role. Allows the use of tags to identify the agent (ex: QA, Staging, Prod, etc.)

- **az_devops_deployment_group_name**

  Use in conjuction with the `deployment` agent role. The name of the deployment group in which to add the agent.

- **az_devops_environment_name**

  Use in conjuction with the `resource` agent role. The name of the environment in which to add the VM resource.

- **az_devops_agent_replace_existing**

  Adds the `--replace` argument to the configuration script for the [scenario where you need to replace an exiting agent with a new host](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops#pool-and-agent-names).

- **az_devops_reconfigure_agent**

  Forces a reconfiguration of the agent even if the service is already active

- **az_devops_agent_user_capabilties**

  A Dictionary of environment variables to set for the agent process which translate to User Capabilties which can be helpful for setting [release pipeline demands](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/demands?view=azure-devops&tabs=yaml)

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: agents
      roles:
         - yohanb.azure_devops_agent
      vars:
        - az_devops_agent_role: build
        - az_devops_accountname: fubar
        - az_devops_accesstoken: ***

## License

Copyright © 2020, GSoft inc. This code is licensed under the Apache License, Version 2.0. You may obtain a copy of this license at https://github.com/gsoft-inc/gsoft-license/blob/master/LICENSE.
