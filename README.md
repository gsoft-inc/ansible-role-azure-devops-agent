# Role Name

An Ansible role that installs and configures a Linux machine to be used as an [Azure DevOps build or deployment agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=vsts).

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    az_devops_agent_version: 2.142.1
    az_devops_agent_user: "az_devops_agent"
    az_devops_agent_name: "{{ ansible_hostname }}"
    az_devops_server_url: "https://{{ az_devops_accountname }}.visualstudio.com/"
    az_devops_agent_folder: "/home/{{ az_devops_agent_user }}/agent/"
    az_devops_work_folder: "/home/{{ az_devops_agent_user }}/work/"
    az_devops_agent_pool_name: "Default"
    az_devops_agent_role: "build"
    az_devops_deployment_group_tags: null

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

  Agent role, either `build` or `deployment`. The build role adds the agent as a build server. The deployment role adds the agent as a deployment server.

## Dependencies

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

## License

BSD

## Author Information

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
