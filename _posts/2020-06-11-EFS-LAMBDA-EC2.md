---
layout: post
title:  "EFS, Lambda and EC2 a simple Intro"
date:   2020-07-11 13:28:34 +1000
categories: EFS and Lambda, your first intro
---

On the 18th of June 2020 AWS released EFS for Lambda.  This is a pretty great release.  This provides serverless architectures a way to implement common tasks such as large libraries ( lambda is limitd 50mb per funciton ), access to files your other servers may have access to eg assets from exisitng applications or the ability to drop assets for comsuption for internal or exisitng services.  If this quick walth through we will look at how to creat your first Elastic File System, how to mount it in EC2 and how to mount it to Lambda and demostrate traditional compute accessing the same files as lambda.

This diagram provides an overview:

![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/diagram.jpg "Diagram")

The first thing we will do is define a Security groups:

1.  You home computer/laptop ssh to ec2


Nvaigate in the console to:

```
https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#SecurityGroups:
```

Click "Create security Group" and fill in the details as so:


![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/homessh.png "Diagram")

Next we will spin up an ec2 instance and ssh to the machine.

Navigate to:

```
https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#LaunchInstanceWizard:
```

Choose Amazon Linix 2 AMI, 64bix (x86), click "Select".  On the next screen select "t2.micro" then click "Configure instance details", ensure "Auto-assign Public IP" is set to enable as we want to ssh t directly to this machine.

![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/ec2.png "Diagram")

Click "Next" Add storage", Click "Next: Add tags", Click "Next: Configure Security Group".  We want to use the secutiyrt group we created, click "Select an <strong>existing</strong> security group." Select teh security group we just created.

![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/selectsg.png "Diagram")

Click "Review and Launch", then click "Launch", make sure you select an existing key pair or create a new one.

We will ssh to the machine to test:

```
chmod 400 ~/pathto/sshkey.pem

ssh -i ~/pathto/sshkey.pem ec2-user@ipaddress
```

You can find the IP address by viewing the information panel at:

```
https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#Instances:sort=instanceId
```

Once you can ssh to the ec2 instance we are ready to create the Elastic File System.

Now that we have a secuirty group and ec2 instance we want to allow the ec2 instacne to connect to EFS, we will create a secuity group for this purpose.

Again navigate to:

```
https://ap-southeast-2.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-2#SecurityGroups:
```

Click "Create security Group" and fill in the details as so.  The sourge is the secuirty group we created for access to the ec2 instance, so type "EC2-SGroup" and select that item.

![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/sg2.jpg "Diagram")


  Navativate to:
```
https://ap-southeast-1.console.aws.amazon.com/efs/home?region=ap-southeast-1#/filesystems
```

And click "Create file system" fill out the details as so and click "Sext Step", ensure you use the EFS security group we created, this will allow resources to access the EFS.  Click "Next Step"

![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/fsnetwork.png "Diagram")

Leave the defaults as so:
![Diagram](/assets/post/2020-06-11-EFS-LAMBDA-EC2/fs.png "Diagram")


TBC