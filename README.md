# sonatype-nexus3-ansible

Ansible playbook for installing Sonatype Nexus 3 (Repository Manager).

Reference: https://help.sonatype.com/repomanager3/installation/system-requirements

> \* This is an Ansible playbook that was actually used in the past.
> It records the environment at that time, and does not guarantee that it can be used as-is with the current Nexus / CentOS / Ansible environments.

## Environment

- OS: CentOS 8 (KVM guest)  
  \* It was later upgraded to Rocky 9 and is currently running.

## Procedure

Log in to CentOS 8 as root and perform the following operations.

\* CentOS 8 is assumed to be installed with the minimal configuration.

### Install Ansible and git

```bash
dnf install -y epel-release
dnf install -y ansible git
```

### Transfer the playbook

Transfer this project to the installation target.

### Change settings

Open the file `group_vars/nexus_repository_manager` in the playbook with an editor and make the necessary configuration changes.

### Run the playbook

Move to the transferred `sonatype_nexus3_ansible` directory and run the `ansible-playbook` command.
The installation will start.

```bash
cd sonatype_nexus3_ansible
ansible-playbook -i hosts site.yml
```
