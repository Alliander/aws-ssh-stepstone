# aws-ssh-stepstone

This cloudformation script creates a SSH stepstone/bastion host in aws for secure access to your vpc. It is based upon: <https://cloudonaut.io/manage-aws-ec2-ssh-access-with-iam/> / <https://github.com/widdix/aws-ec2-ssh>. Read it, it explains the concept.


## (Extra) Features:

- SSH users managed by AWS IAM
- IAM group to specifiy which users are allowed root on the stepstone 
- Whitelisting (creates a security group)
- Specify ssh listening port (default 443 to pass trough corporate proxies)
- yum-cron-daily to keep the stepstone up to date
- Elastic IP

## Configuration

The following parameters can be defined:

Parameter | Description | Default
--------- | ----------- | -------
`AmiId` | Id of the AMI latest aws linux ami in your region. See https://aws.amazon.com/amazon-linux-ami/ | none
`CidrWhitelist` | CIDR block for whitelisting incomming traffic. If more needed add them manually to the security group | 0.0.0.0/0
`SshPort` | SSH listening port | 443
`Subnet` | Specify the subnet in which the stepstone will be placed | none
`VPC` | Specify the vpc of the previous selected subnet | none

## Usage

First create users and upload their ssh keys. If they require root on the stepstone, which will be not likely if only used for forwarding and tunneling, add them to the {StackName}-StepstoneRoot group in IAM.

#### Connect to the stepstone using SSH
ssh *iam-username*@*eip-address* -i  */path/to/your/private/key/id_rsa* -p *ssh-port* 

#### For tunneling add
-L *local-port*:*destination-host*:*destination-port* -N

#### When passing trough an outbound corporate proxy add
-o "ProxyCommand=nc -X connect -x *proxyip/fqdn*:8080 %h %p"


Of course you could also use the ssh tunnel manager of your choice or autossh to keep it alive.



