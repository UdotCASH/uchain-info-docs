# Overview

## Playbook Overview

{% hint style="info" %}
The playbook infrastructure currently only supports [AWS](./#using-aws-codedeploy-to-monitor-and-manage-a-uchaininfo-deployment) as a cloud provider. If using another provider, please see [manual deployment](../manual-old-ui/).
{% endhint %}

We use [Ansible](https://docs.ansible.com/ansible/latest/index.html) & [Terraform](https://www.terraform.io/intro/getting-started/install.html) to build the correct infrastructure to run uchaininfo. The playbook repository is located at [https://github.com/poanetwork/uchaininfo-terraform](https://github.com/poanetwork/uchaininfo-terraform).&#x20;

In the root folder you will find Ansible Playbooks to create all necessary infrastructure to deploy uchaininfo. The `lambda` folder also contains a set of scripts that may be useful in your uchaininfo infrastructure.

1. [Deploying the Infrastructure](deploying-the-uchaininfo-infrastructure.md). This section describes all the steps to deploy the virtual hardware that is required for production instance of uchaininfo. Skip this section if you do have an infrastructure and simply want to install or update your uchaininfo.&#x20;
2. [Deploying uchaininfo](deploying-uchaininfo.md). Follow this section to install or update your uchaininfo instance.
3. [Destroying Provisioned Infrastructure](destroying-provisioned-infrastructure.md). Refer to this section if you want to destroy your uchaininfo installation.
