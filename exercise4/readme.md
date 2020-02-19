# SSH 2 Ways

## Setting up a bastion

First let's create a VPC

`aws ec2 create-vpc --cidr-block 10.0.0.0/24 --region ap-southeast-2`

Make a note of the VpcId from the output

Now let's create a subnet

`aws ec2 create-subnet --cidr-block 10.0.0.0/24 --vpc-id <vpc-id>`

Now go into the AWS console, and open `ec2`

Click launch instance

Select the `Amazon Linux 2 AMI (HVM), SSD Volume Type` AMI, it should be the top option

Select `t2.micro` for the instance size

Change the network to the VPC you created

Set `Auto-assign Public IP` to `enabled`

Keep `Next` until you get to `Launch`

On the Key Pair modal that appears, click `Create a new key pair`

Enter a name for your key pair and click `Download Key Pair`

## Certificate SSH

We downloaded a PEM file, which has the certificate you need to access the machine, first we need to fix the permissions on the file

`chmod 400 <path-to-your-downloaded-pem>`

Now get the public IP address of the machine from the AWS console

And we **should** be able to ssh to the box

`ssh -i <path-to-downloaded-pem> ec2-user@13.54.74.247`

But we can't, why can't we?

The answer is the machine can't route back to us, we need an internet gateway attached to the VPC

Open the AWS console, and open the VPC service page

Select `Internet Gateway` on the left hand side, and click `Create Internet Gateway`

Once that is done, select the Internet Gateway you made, click `Actions` and select `Attach to VPC`

Now we need to configure the routing, select `Route Tables` on the left hand side

Select the route table for your VPC

Click `Routes` in the bottom of the screen, and then click `Edit Routes`

Add a new route with destination `0.0.0.0/0` and set the target as the Internet Gateway

Now if we try to SSH again, we should have success!

As we're connecting to the machine for the first time, it'll say we don't recognise the authenticity of the host

If we type `yes` then we should be on the box!

## IAP SSH

In order to SSH as we did before, the machine needed a public IP address, but we can do better.

Now you will need to install a AWS CLI plugin, follow the instructions [here](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html#install-plugin-macos)

You may be wondering how we're going to talk to the jump box without a public IP address, surely we have to create a VPN or equivalent to make it addressable.

Actually in his case we're going to enable `Endpoints` and `PrivateLink` for our VPC, which means we can talk to AWS services directly from the box

So run the following two commands substituting your VPC id:

```bash
aws ec2 modify-vpc-attribute --enable-dns-support --vpc-id <vpc-id>
aws ec2 modify-vpc-attribute --enable-dns-hostnames --vpc-id <vpc-id>
```

Now open up the AWS console and open up the VPC service page

Click Security Groups on the left hand side, then `Create Security Group`

Give it a name, a description and select your VPC

Then select the Security Group, click `Inbound Rules` at the bottom and then `Edit Rules`

Add a rule that allows HTTPS traffic from `10.0.0.0/24`

Then click `Endpoints` on the left hand side, and then `Create Endpoint`

Select the Service Name `com.amazonaws.ap-southeast-2.ssmmessages`

Select your VPC and the security group you just created

Now do the same from the `com.amazonaws.ap-southeast-2.ssm` service

Create a machine as you did before, but this time don't give it a public IP address

Now open IAM, go to `Roles` and click `Create Role`

Select `EC2` and click `Next`

Search for roles including the letters `SSM` and select `AmazonSSMAutomationRole`

Click next twice, and then give your role a memorable name and click `Create Role`

Create an EC2 machine as you did before, however this time:

- Don't give it a public address
- For `IAM Role` give it the one you just created

Now if you run `aws ssm start-session --target <your-instance-id>` you should get dropped into a shell on the box
