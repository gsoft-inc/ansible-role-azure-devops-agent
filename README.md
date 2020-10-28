# Azure DevOps Agent

An Ansible role that installs and configures a Linux machine to be used as an [Azure DevOps build or deployment agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops).

See [this blog post](https://medium.com/gsoft-tech/easily-configuring-an-azure-devops-agent-with-ansible-fb9cb0f98b73) for more detail.

## Requirements

[See prerequisites](https://github.com/Microsoft/azure-pipelines-agent/blob/master/docs/start/envlinux.md)

Installing on MacOS can be problematic when trying to use an admin user for connecting and another user for running the service.
pipelining = True can help, especially if you run into issues where the devops agent user cannot access the temporary files ansible is creating.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
    az_devops_accountname: null
    az_devops_accesstoken: null
    az_devops_project_name: null
    az_devops_agent_version: 2.168.1
    az_devops_agent_user: "az_devops_agent"
    az_devops_agent_name: "{{ ansible_hostname }}"
    az_devops_server_url: "https://dev.azure.com/{{ az_devops_accountname }}"
    az_devops_agent_folder: "/home/{{ az_devops_agent_user }}/agent/"
    az_devops_work_folder: "/home/{{ az_devops_agent_user }}/agent/_work"
    az_devops_agent_pool_name: "Default"
    az_devops_agent_role: "build"
    az_devops_deployment_group_tags: null
    az_devops_environment_name: null
    az_devops_deployment_group_name: null
    az_devops_agent_replace_existing: false
    az_devops_reconfigure_agent: false
    az_devops_agent_user_capabilties: null
    az_devops_proxy_url: null
    az_devops_proxy_username: null
    az_devops_proxy_password: null
```

- **az_devops_accountname**

  The name of your Azure DevOps account, i.e. https://dev.azure.com/YOUR_ACCOUNT_NAME

- **az_devops_accesstoken**

  The Personal Access Token (PAT) used to authenticate to your account. [See here for details on how to generate this value](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=vsts#authenticate-with-a-personal-access-token-pat).

  _Note: Think about using Ansible Vault to secure this value._

- **az_devops_project_name**

  The name of the Azure DevOps project in which to register the agent (only used for deployment groups).

- **az_devops_agent_version**

  Version of the installed agent package. Should be periodically updated to the latest version (see [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=vsts#download-and-configure-the-agent))

- **az_devops_agent_user**

  Name of the user used to run and configure the service.

- **az_devops_agent_group**

  Default group of the user used to run and configure the service.

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

  Use in conjuction with the `deployment` agent role. The name of the deployment group in which to add the agent.  **This needs to be manually created in you Azure DevOps project beforehand.**

- **az_devops_environment_name**

  Use in conjuction with the `resource` agent role. The name of the environment in which to add the VM resource.  **This needs to be manually created in you Azure DevOps project beforehand.**

- **az_devops_agent_replace_existing**

  Adds the `--replace` argument to the configuration script for the [scenario where you need to replace an exiting agent with a new host](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops#pool-and-agent-names).

- **az_devops_reconfigure_agent**

  Forces a reconfiguration of the agent even if the service is already active

- **az_devops_proxy_url**

  The URL of the proxy server, format is `http://url:port`

  This assumes the proxy does both http and https

- **az_devops_proxy_username**

  Username for the proxy

  If the proxy does not require authentication, then just leave defaults

- **az_devops_proxy_password**
  
  Password for the proxy

  Again if proxy does not require authentication, just leave the defaults.
  
  _Note: Think about using Ansible Vault to secure this value._

- **az_devops_agent_user_capabilties**

  A Dictionary of environment variables to set for the agent process which translate to User Capabilties which can be helpful for setting [release pipeline demands](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/demands?view=azure-devops&tabs=yaml)

  Example usage:

```yaml
    - az_devops_agent_user_capabilties:
      user_capabilty_key: user_capability_value
```

## Example Playbooks

### Build Agent

```yaml
- hosts: agents
  roles:
      - gsoft.azure_devops_agent
  vars:
    - az_devops_agent_role: build
    - az_devops_accountname: fubar
    - az_devops_accesstoken: ***
```

### Deployment Agent

```yaml
- hosts: agents
  roles:
      - gsoft.azure_devops_agent
  vars:
    - az_devops_agent_role: deployment
    - az_devops_accountname: fubar
    - az_devops_accesstoken: ***
    - az_devops_project_name: baz
    - az_devops_deployment_group_name: fubar_group
    - az_devops_deployment_group_tags: "web,prod"
```

### Resource

```yaml
- hosts: agents
  roles:
      - gsoft.azure_devops_agent
  vars:
    - az_devops_agent_role: resource
    - az_devops_accountname: fubar
    - az_devops_accesstoken: ***
    - az_devops_project_name: baz
    - az_devops_environment_name: staging
```

### Proxy

```yaml
- hosts: agents
  roles:
      - gsoft.azure_devops_agent
  vars:
    - az_devops_agent_role: build
    - az_devops_accountname: fubar
    - az_devops_accesstoken: ***
    - az_devops_proxy_url: "http://127.0.0.1:8080"
    - az_devops_proxy_username: bob
    - az_devops_proxy_password: ***
```

## License

Copyright © 2020, GSoft inc. This code is licensed under the Apache License, Version 2.0. You may obtain a copy of this license at https://github.com/gsoft-inc/gsoft-license/blob/master/LICENSE.
