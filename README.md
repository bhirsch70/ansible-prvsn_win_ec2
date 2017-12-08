# ansible-prvsn_win_ec2
Create or Terminate Windows servers in AWS EC2

## Summary
This playbook was written to allow you to create or terminate a Windows Server(s) instance quickly in AWS EC2.  It creates a custom Security Group, creates the instance, then waits for `winrm` to respond.  When terminating an instance, it leaves the Security Group in place.

## Prerequisities
- You must have an AWS account
- You must have pre-created a VPC and subnet, the Security Group will be created during the provisioning
- Creation of an ansible-vault encrypted string to seed the `win_init_pass` variable.  See [ansible-vault encrypt_string](https://docs.ansible.com/ansible/2.4/vault.html#use-encrypt-string-to-create-encrypted-variables-to-embed-in-yaml)

## Example Playbook execution
`ansible-playbook create.yml --tags="create" --ask-vault-pass`

`ansible-playbook terminate.yml --tags="terminate" --ask-vault-pass`

### Extra Vars
This playbook uses an extra variables file.  By setting your own AWS-custom variables this way, you can avoid having to mess with the role itself.

| variable name | description | example |
|---------------|-------|---------|
|ec2_type| Instance type |t2.small|
|ec2_image| Windows Server Amazon machine image (defaults to Server 2016) |ami-08910872|
|ec2_region| Amazon region |us-east-1|
|ec2_vpc_id| Pre-built VPC |vpc-2a994a4c|
|ec2_vpc_subnet_id| Pre-built subnet |subnet-b70447fe|
|ec2_instance_name| Instance Name |ansible-windows|
|ec2_instance_prefix| Prepend  Instance Name with this |BH-DEMO|

## role
ansible-role-prvsn_win_ec2

###  default vars

| variable name | description | default |
|---------------|-------|---------|
|ansible_connection| method of connection |winrm|
|ansible_ssh_port| port to connect  |5986|
|ansible_ssh_user| User Account |Administrator|
|ec2_wait| wait for ec2 instance |yes|
|ec2_exact_count| # of instances to exist |1|
|win_init_pass| ansible-vault encrypted string for initial Windows Administrator password |N/A|
|ansible_ssh_pass| ansible password to use to login to new Windows instance |"{{ win_init_pass }}"|

## Example playbook (create.yml)
```
---
- name: Create lab instances in AWS
  hosts: localhost
  connection: local
  become: no
  gather_facts: no

  roles:
    - ansible-role-prvsn_win_ec2
```

### Tags
This playbook uses tags to target the tasks appropriate to the operation

`create` - targets only the playbook tasks required during instance creation

`terminate` - targets only the playbook tasks required during instance termination
