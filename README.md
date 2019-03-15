# cloud-gcp

This intermediate-level Ansible playbook example demonstrates for provisioning an instance group, two virtual machines, and some supporting items into a new Google Cloud Platform VPC. Once provisioned this example will use an official third party role from Ansible Galaxy to install and start nginx.

This example demonstrates a few other best practices and intermediate techniques:

* **Compound Playbook.** This example shows the use of a playbook, `site.yml`, combining other playbooks, `provision-*.yml` and `install-nginx.yml`, using play level 'include' statements. Splitting provisioning and configuration plays into separate files is a recommended best practice. By doing so, users have more flexibility to what the run without needing tags and wrapping blocks of tasks with conditionals. This separation can be used to promote reuse, but enabling the mixing and matching of various playbooks.
* **Controlling Debug Message Display.** There is a debug task in `site.yml` using the optional `verbosity` parameter. Avoiding the unnecessary display of debugging messages avoids confusion and is just good hygiene. In this implementation, the debug message won't be displayed unless the playbook is run in verbose mode level 1 or greater.

## Requirements

To use this example you will need a couple things

* There has to be a GCP project created to put things in.
* You machine needs the Google SDK installed and logged in (gcloud init)
* Ansible needs a GCP service account setup and properly configured on your control machine.
* the google-auth module Python installed. See the [Ansible GCE Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_gce.html) for more details.
* Have connected to a host using "gcloud compute ssh" at some point in the past using the same user you are running Ansible as (so SSH keys are setup).
* You can NOT run this as root, GCP does not like remote users connecting as root


## Variables

Before running this playbook example, you should know about what variables it uses and how they effect the execution of the GCP provisioning process.

### Required

The following variables must be properly set for this example to run properly.

* `gcp_project`: The GCP project you will be deploying into

* `gcp_auth_file`: The credential file (in JSON format) that is created from the GCP service account console.

## Usage

First step is to get the role from Ansible Galaxy for nginx.

```
ansible-galaxy install nginxinc.nginx
```

To execute this example, use the `site.yml` playbook and pass in all the required variables:

```
ansible-playbook site.yml -e "gcp_project=lightbulb gcp_auth_file=/home/ansible/gcp.json"
```

Using verbose mode of any type will emit a debugging message that displays information about the provisioned instances in the stack.

```
ansible-playbook site.yml -e "gcp_project=lightbulb gcp_auth_file=/home/ansible/gcp.json" -vvvv
```

Remember you can put your variables in a YAML formatted file and feed it into the play with the @ operator.

```
ansible-playbook site.yml -e @extra_vars.yml
```

