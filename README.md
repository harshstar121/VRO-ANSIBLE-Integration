Execute VRO workflow from Ansible
====================================================================

Ansible is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality.

Using the ansible playbook with set of tasks make it easier to execute the VRO workflow. We're using the **uri** module to call the VRO REST APIs.


Related
----------------------------------------------

* [Ansible Role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)
* [uri module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html)
* [Ansible CLI](https://docs.ansible.com/ansible/latest/user_guide/command_line_tools.html)
* [vRealize Orchestrator REST API](https://docs.vmware.com/en/vRealize-Orchestrator/7.6/com.vmware.vrealize.orchestrator-develop-web-services.doc/GUID-E767F363-6FEA-4998-8119-9C51C5A9C73A.html)


### Ansible Role ###
Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content in roles, you can easily reuse them and share them with other users.

Created a role called **execute-vro-workflow**
which can
* check if the workflow exist
* Execute the workflow
* wait and check the execution status of workflow.
* show error if workflow is failed.

##### Role Structure #####
```
root
│   README.md
│   execute-vro-workflow.yml
└──execute-vro-workflow   
│  └──defaults
│      │ main.yml
│  └──tasks
│      │ check-workflow.yml
│      │ execute-workflow.yml
│      │ main.yml
│  └──templates
│      │ workflow-input.json
```
##### Playbooks and Files #####
* execute-vro-workflow.yml - Top entry level playbook to execute the role.
* defaults/main.yml - contains the defaults parameters like VRO url, credentials, workflow_id.
* tasks/main.yml - segregate the logic to execute other playbooks.
* tasks/check-workflow.yml - contains the logic to check if workflow is exist.
* tasks/execute-workflow.yml - contains the logic to execute the workflow and check the status.
* templates/workflow-input.json - workflow input in json format for REST call.

##### Inputs #####
| Variable Name        | Description                    | Defaults                    |
|----------------------|--------------------------------|-----------------------------|
| vro_server_url       | Provide the FQDN of VRO        | stored in defaults var      |
| vro_workflow_id      | Workflow ID to execute         | stored in defaults var      |  
| vro_server_user      | User account for REST API      | stored in defaults var      | 
| vro_server_pwd       | Password for the same account  | stored in defaults var      |  
| wf_yourName          | Workflow input-parameter       | runtime input as extra-vars | 


The workflow I'm using has only one input-parameter called `yourName` and to differenciate it I'm using a local variable as `wf_yourName`
which is also part of templates/workflow-inputs.json (json payload for REST API)

`NOTE: Need to create as many inputs as the VRO workflow contains.`


Execution
----------------------------------------------
This role can be executed via ansible command
```
ansible-playbook <playbook_name>.yml --extra-vars "<var_name>=<var_value>"

Example.
ansible-playbook execute-vro-workflow.yml --extra-vars "yourName=Harshit"
```

Author
-------
Harshit Kesharwani

