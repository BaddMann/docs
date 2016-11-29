# Zentral - Amazon AWS 

The Amazon AWS based setup is quick way to start Zentral.

## Intro
This document will help you to start [Zentral](https://github.com/zentralopensource) on **Amazon AWS**. 

For setup we use Amazon AWS web interface to launch a EC2 instance from a pre-build AMI.

*Note: We also provide a guide for a Google Cloud based setup - please look [here]().*

## Overview

This is a guide to run a fully working [Zentral](https://github.com/zentralopensource/zentral) on **Amazon AWS**. 
We show how to launch a instance of Zentral based on our publicly available **Zentral all in one** pre-build AMI (Amazon image). The pre-build image is build with Packer, Ansible is used as provisioner. 

1. It's useful to have the AWS documentation at hand, please start to look into the [Getting Started with Amazon EC2 ](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) and read about [Security Groups](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html).

2. Read the requirements - you need to be able locate our public AMI from available region, use AWS created ssh-keys, set up a security group and choose the right Instance Type for EC2. 

3. Read the intersection on configuration files in Zentral. Details are important, we strongly recommend to change the default htaccess protection in `zentral.htpasswd` file, and you may want to enable integrations for 3rd party services in the `base.json` file.

4. We provide a sequence of steps in the AWS EC2 section and launch a pre-build "Zentral all in one" AMI. 

5. Read the  overview of **Zentral all in one** build in CLI tools. These are available in `/home/zentral/app/utils/` once you connect via ssh to the running instance on AWS.

*Note: Please keep in mind this tutorial is a starting point, a full walk thru all options with AWS EC2, VPC, etc. is obviously out scope.*

## Requirements:

You need to have the following already in place to start deployment of **Zentral all in one**  on AWS. 

- Active AWS account (start here: https://console.aws.amazon.com)
- Look into EC2 Service > Images > public AMI, search "Zentral all in one" available in the AWS region
- Depening where you located you may switch AWS regions (i.e. N.Virginia, Frankfurt) to locate the public AMI
- You must have created a key-pair in AWS to connect to the AMI
- A t2.medium (or larger) Instance Type is recommended
- Username to connect with is "ubuntu" (AWS default ubuntu)
- Ports in security group must be opened for 443, 80, 22
- AWS AMI assigned external IP address must be set in your DNS (so Let's Encrypt will work)


*Note: A free tier eligible t2.micro instance is available in AWS, please do not use this for this tutorial. Unfortunately the t2.micro instance type has just 1 GB Ram which is not enough to reliably run Zentral, ElasticSearch and Kibana 5 all in one box*

## Edit configuration

To add or change `zentral` configuration you need to ssh into your instance, change into the directory with `cd /home/zentral/conf`. You may edit the following files here:

- `base.json` - edit here for the main configuration like adding actions, integrations, etc. 
- `contacts.json` edit here for contacts of admins, admin-groups to notify via email, SMS
- `zentral.htpasswd` edit here for change the password htaccess protected Zentral web interface, Kibana, Prometheus


The Zentral configuration options are used to enable integration with 3rd party tools. A simple addition to the `base.json` configuration file will enable actions/notifications. This will enable custom actions for Probes to trigger notifications to Slack, add cards to a Trello board, create a ticket in Zendesk, etc.
Providing URL and tokens in the `base.json` configuration file is required to connect the *Zentral Inventory worker* via REST API and fetch inventory data and/or change group membership in JAMF Pro, Watchman Monitoring, and Sal.

## AWS / EC2 setup

We have previously covered AWS setup in a tutorial video, please also look [here](https://goo.gl/2hFN4X) in case you struggle to follow the instructions.
We have example screenshots for AWS setup [here](https://github.com/zentralopensource/docs/blob/master/images_aws), you can follow them along this document.

### AWS Management Console

**Log into AWS Console**
1. Log into your AWS Management Console
2. Go to EC2 Service 
3. See the EC2 Overview section

**Locate AWS region with Zentral public AMI available**

1. In EC2 Service go to Images section (left side bar) > click AMIs
2. Change the search to Public Images
3. Search for "Zentral all in one" 
4. Repeat in other AWS regions if you don't find it

*Note: "Zentral all in one" AMI is not available in all AWS regions,
we uploaded it to popular regions (N.Virginia, Frankfurt).*

**Create a Key-Pair**

1. In EC2 Overview > Resources click on Key Pairs
2. Click "Create Key Pair" button 
3. Set a Key pair name (like "ssh-key-zentral-aws")
4. Your Key Pair should be available for download as .pem or .pem.txt file (i.e. ssh-key-zentral-aws.pem)

**Edit the Security Group**

1. In EC2 Overview > Resources click on Security Groups
2. Click "Create Security Group" button
3. Set a Security group name
4. Create Inbound rules with "Add Rule" button
5. Create a Custom TCP Rule, Port Range **443**, Source **Anywhere**
6. Next create a Custom TCP Rule, Port Range **80**, Source **Anywhere**
7. Next create a Custom TCP Rule, Port Range **22**, Source **Anywhere**

*Note: You may want to adjust/extend the Rules for your security needs (i.e. set Port 22, Source: MyIP)*

**Launch the AMI**

1. In EC2 Service go to Images section (left side bar) > click AMIs
2. Change the search to Public Images
3. Search for "Zentral all in one" 
4. Select the AMI "Zentral all in one"
5. Click the "Launch" button
6. Choose t2.medium as your Instance Type (or use t2.large)
7. Click the "Review and Launch" button
8. Read the Notes before you agree (note: you get charged for t2.medium instances)
9. Click the "Edit security groups" link
10. For "Assign a security group" pick option to "Select an existing security group"
11. Select your Security Group created (Ports 443,80,22)
12. Review Instance Launch section again (you can open details in the web view)
13. Click the Launch button to start the instance
14. Select existing key pair, then press "Launch Instances" button 
15. Click
16. Wait for the Instance State is "running"
17. Look for the Public IP assigned to your instance

**Set the DNS **

1. Go to your DNS provider 
2. Set up an "A Record" with the public IP of your instance (i.e. "zentral.example.org" for the domain "example.org")

**SSH into your instance**
1. Locate the Key Pair 
2. Set POSIX permissions 600 for your key-pair
3. Open the Terminal and follow the instruction below to connect via ssh to your instance 
4. Remember the name to login via SSH is "ubuntu"

**Setup Let's Encrypt TLS certs for your instance**

1. Make sure you've set IP address in your public DNS, create an *A record* to create a FQDN like *zentral.example.com* 

2. Use the following command, provide your FQDN as parameter```bash
ssh -i .ssh/ssh-key-zentral-aws.pem ubuntu@zentral.example.org sudo /home/zentral/app/utils/set_fqdn.py zentral.example.org
```
or if you are logged in already use 
`sudo /home/zentral/app/utils/set_fqdn.py zentral.example.org`

3. You'll see feedback in Terminal about a unique **dhparam** and Let's Encrypt
certificate created

4. Tips to troubleshoot, just in case things have gone wrong somewhere: 
  - Check ports 80/443 opened inbound in your AWS Security Group 
  - You can resolve the FQDN / external IP
  - Check you provide the right FQDN as parameter and used `sudo` privileges
  - In case you ran the command multiple times, check for conflicting Nginx conf files in `/etc/nginx/conf.d/`


**Open Zentral in your Browser**

1. Start your favourite browser and navigate to your FQDN (i.e. zentral.example.com) 

2. The access is protected by `htaccess`  use  **test** /  **test** to get initial access

3. Congratulations, you should see the Zentral web interface now

4. We recommend to change htaccess immediately (before you go on and enroll any clients)

## SSH / Terminal commands

Connect via ssh to your instance 

Simple SSH connection
`sh -i .ssh/ssh-key-zentral-aws.pem ubuntu@zentral.example.org`

To create a valid TLS certificate from Let's Encrypt launch the `set_fqdn.py`script, provide your server FQDN (i.e. sudo ../set_fqdn.py zentral.example.org)

```bash
ssh -i .ssh/ssh-key-zentral-aws.pem ubuntu@zentral.example.org sudo /home/zentral/app/utils/set_fqdn.py zentral.example.org
```
**Example from **
```bash
# launch set_fqdn.py script, provide your server FQDN (i.e. sudo ../set_fqdn.py zentral.example.org)
ubuntu@zentral-demo:~$ sudo /home/zentral/app/utils/set_fqdn.py zentral.example.org

Generating a 2048 bit RSA private key
........................................................+++
........................................+++
writing new private key to '/tmp/zentral.example.org/ssl/key.key'
-----
2016-11-28 10:42:01,625:WARNING:letsencrypt.client:Registering without email!

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/zentral.example.org/fullchain.pem. Your cert
   will expire on 2017-02-26. To obtain a new version of the
   certificate in the future, simply run Let's Encrypt again.
 - Your account credentials have been saved in your Let's Encrypt
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Let's
   Encrypt so making regular backups of this folder is ideal.
 - If you like Let's Encrypt, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

Generate new dhparam ...
Generating DSA parameters, 4096 bit long prime
..............+..+......+.+.......+............................+..................+....+......+....................+.........+...+..+........+....+......+......+...........................+.+........+..................+..+............+.....+.........+.....+.+............................+++++++++++++++++++++++++++++++++++++++++++++++++++*
........+..........+...+.....+..+........+......+.......+...+....+......+.+...+.............+........+...............+++++++++++++++++++++++++++++++++++++++++++++++++++*
New dhparam generated
```

Fix permissions for your key pair in case you experience a error like below
```
 ssh -i /Users/name/Downloads/ssh-key-zentral-aws.pem ubuntu@zentral.example.org                                  
The authenticity of host 'zentral.example.org (54.146.xxx.xxx)' can't be established.
ECDSA key fingerprint is SHA256:1K8T+DPaJxxXLFoA/jEQ2OxKP86jylY1Wc6w2xAivC4.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'zentral.example.org,54.146.xxx.xxx' (ECDSA) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/Users/name/Downloads/ssh-key-zentral-aws.pem ' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/Users/name/Downloads/ssh-key-zentral-aws.pem ": bad permissions
Permission denied (publickey).

# fix with 
chmod 600 /Users/name/Downloads/ssh-key-zentral-aws.pem 

## Zentral - build in CLI tools

Here are useful tools and commands to verify Zentral is running, start a code upgrade directly downloaded from Zentral GitHub repo, restart after a `base.json` configuration change.


For a complete restart of `zentral` :

```bash
sudo /home/zentral/app/utils/reload_restart.sh 
```

Edit htaccess file:

```bash
sudo nano /etc/nginx/zentral.htpasswd
```

Zentral code update pulled from github :

```bash
sudo /home/zentral/app/utils/deploy.py


### example run ###
ubuntu@ip:~$ sudo /home/zentral/app/utils/deploy.py 
Already on 'master'
Your branch is up-to-date with 'origin/master'.
remote: Counting objects: 15, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 15 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (15/15), done.
From https://github.com/zentralopensource/zentral
 * branch            master     -> FETCH_HEAD
   dece953..77facef  master     -> origin/master
Updating dece953..77facef
Fast-forward
 server/templates/inventory/archive_machine.html                          | 32 ++++++++++++++++++++++++++++++++
 server/templates/inventory/machine_detail.html                           |  1 +
 zentral/contrib/inventory/migrations/0009_machinesnapshot_archived_at.py | 20 ++++++++++++++++++++
 zentral/contrib/inventory/models.py                                      | 16 +++++++++++++---
 zentral/contrib/inventory/urls.py                                        |  1 +
 zentral/contrib/inventory/views.py                                       | 22 ++++++++++++++++++++--
 6 files changed, 87 insertions(+), 5 deletions(-)
 create mode 100644 server/templates/inventory/archive_machine.html
 create mode 100644 zentral/contrib/inventory/migrations/0009_machinesnapshot_archived_at.py
/home/zentral/app/releases/2016.09.29-14.17.41-master-77face
...
Operations to perform:
  Apply all migrations: contenttypes, inventory, munki, osquery, probes, sessions
Running migrations:
  Applying inventory.0009_machinesnapshot_archived_at... OK
```

List status for Zentral apps and workers:

```bash
sudo systemctl status zentral_web_app
sudo systemctl status zentral_inventory_worker
sudo systemctl status zentral_store_worker
sudo systemctl status zentral_processor_worker
```
