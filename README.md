# ansible-role-prvsn_win_ec2
Provision Windows servers in AWS

## Summary
This playbook and role will create or terminate a Windows Server instance in AWS Ec2

## Prerequisities
- You must have an AWS account
- You must have pre-created a VPC and subnet

## Extra Vars
This playbook requires an custom file for extra variables and by setting you variables this way, you can avoid having to mess
with the role itself.

| variable name | value |
|---------------|-------|
| ec2_vpc_id | Set this to your own VPC ID |

