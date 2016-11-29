# Zentral - Google Cloud Services

The Google Cloud based setup is quick way to start Zentral.

## Intro
This document will help you to start [Zentral](https://github.com/zentralopensource) on **Google Cloud**. 

For setup we use the `gcloud` command line tool, the tool is installed as part of the the Google Cloud SDK. The Google Cloud SDK is available [here](https://cloud.google.com/sdk/)

*Note: We also provide a guide for Amazon AWS, please look [here](<https://github.com/zentralopensource/docs/blob/master/zentral-aws-setup.md>).*

## Overview

This is a guide to run a fully working [Zentral](https://github.com/zentralopensource/zentral) on Google Cloud. 
We show how to create a instance of Zentral based on our publicly available **Zentral all in one** pre-build image. The pre-build image is build with Packer, Ansible is used as provisioner. 

1. It's useful to have the Google Cloud documentation at hand, please start to look into the [Quickstarts](https://cloud.google.com/sdk/docs/quickstarts) and know about the `gcloud` CLI [Reference](https://cloud.google.com/sdk/gcloud/reference/).

2. Read the requirements - you need to setup a Project, enable the Google Compute Engine API in the Google Cloud Web Console, use the `gcloud` CLI tool installed with the Google Cloud SDK. 

3. Read the intersection on configuration files in Zentral. Details are important, we strongly recommend to change the default htaccess protection in `zentral.htpasswd` file, and you may want to enable integrations for 3rd party services in the `base.json` file.

4. We provide a sequence of commands from *init* to *instances create*. 
The interaction with `gcloud` may vary a little depending your Google Cloud Developer account setup.

5. 5. Read the  overview of **Zentral all in one** build in CLI tools. These are available in `/home/zentral/app/utils/` once you connect via ssh to the running instance on Google Cloud.

*Note: Please keep in mind this tutorial is a starting point, a full walk thru all options in Google Cloud is obviously out scope.*

## Requirements:

You need to have the following already in place to start deployment of **Zentral all in one**  on Google Cloud. 

- Active Google Cloud account (start here: <https://cloud.google.com>)
- Google Cloud SDK installed, with `gcloud` Tool available in Terminal (start here: <https://cloud.google.com/sdk/>) 
- An active Project i.e. "zentral-demo" created in developers console (start here: <https://console.developers.google.com/project>)
- Activated the **Google Compute Engine API** for your Project (<https://console.developers.google.com/apis/api/compute_component/>)
- gcloud assigned external IP address must be set in your DNS (so Let's Encrypt will work)


*Note: Google Cloud provides great a 60 days demo with credit included - this is a great option to start exploring Zentral as well as Google Cloud services*

## Edit configuration

To add or change `zentral` configuration you need to ssh into your instance, change into the directory with `cd /home/zentral/conf`. You may edit the following files here:

- `base.json` - edit here for the main configuration like adding actions, integrations, etc. 
- `contacts.json` edit here for contacts of admins, admin-groups to notify via email, SMS
- `zentral.htpasswd` edit here for change the password htaccess protected Zentral web interface, Kibana, Prometheus


The Zentral configuration options are used to enable integration with 3rd party tools. A simple addition to the `base.json` configuration file will enable actions/notifications. This will enable custom actions for Probes to trigger notifications to Slack, add cards to a Trello board, create a ticket in Zendesk, etc.
Providing URL and tokens in the `base.json` configuration file is required to connect the *Zentral Inventory worker* via REST API and fetch inventory data and/or change group membership in JAMF Pro, Watchman Monitoring, and Sal.

## Google Cloud / gcloud setup

We prefer the `gcloud` CLI setup and provide examples in this document.
Of course Google Cloud setup is also possible in Google Cloud Console GUI, with exeption that you mus upload **Zentral all in one** pre-build image using provided `gcloud` command. We have example screenshots for Google Cloud setup [here](https://github.com/zentralopensource/docs/blob/master/images_gcloud), you can follow them along this document. 

### Google Cloud Console

Prepare a Project in Google Cloud Console

1. Log into the Google Developer Console and create/select a project.
2. Under the Dashboard section, click Enable API.
3. For Google Compute Engine API section, click Enable API.


### gcloud - command line tool in Terminal


**Project Init**
1. Start your setup with `gcloud init` 
2. You need to log into your Google user account:
```bash
# On very first run you'll see this
To continue, you must log in. Would you like to log in (Y/n)? Y

# On sequential run, you may see other options here
Welcome! This command will take you through the configuration of gcloud.
Settings from your current configuration [zentral-demo] are:
...
Pick configuration to use:
 [1] Re-initialize this configuration [zentral-demo] with new settings 
 [2] Create a new configuration
Please enter your numeric choice:
```

**Copy the image to your Project**

1. Start to copy the public image to your Account/Project, use the following command
`gcloud compute images create zentral-all-in-one  --source-uri gs://zentralopensource/zentral-all-in-one.tar.gz`

2. The copy process may take a few minutes (average 3-5min)

**Setup Firewall rules**

1. Open HTTP/HTTPS ports in Firewall, use the following command
`gcloud compute firewall-rules create default-http-https  --description "Incoming http and https allowed." --allow tcp:80,tcp:443`

2. Verify open ports in Firewall, use the following command
`gcloud compute firewall-rules list`

**Start your Instance**

1. Start the instance, make sure you provide names matching your project
`gcloud compute instances create zentral-demo --image-project=zentral-demo --image=zentral-all-in-one`

2. You'll likely need to pick a zone, ensure you use the same zone as your Project.

3. The creation and boot process may take a few minutes (average 2-3min).

4. Note the external IP address for your instance, you'll need this for DNS/FQDN setup.

**Connect to your instance**

1. SSH login to your instance, use the following command
	`gcloud compute ssh ubuntu@zentral-demo`   

2. You'll likely need to pick a zone (again), ensure you use the same zone as before

**Setup Let's Encrypt TLS certs for your instance**

1. Make sure you've set IP address in your public DNS, create an *A record* to create a FQDN like *zentral.example.com* 

2. Use the following command, provide your FQDN as parameter
`sudo /home/zentral/app/utils/set_fqdn.py zentral.example.org`

3. You'll see feedback in Terminal about a unique **dhparam** and Let's Encrypt
certificate created

4. Tips to troubleshoot, just in case things have gone wrong somewhere: 
	- Check ports 80/443 opened in Firewall 
	- You can resolve the FQDN / external IP
	- Check you provide the right FQDN as parameter and used `sudo` privileges
	- In case you ran the command multiple times, check for conflicting Nginx conf files in `/etc/nginx/conf.d/`


**Open Zentral in your Browser**

1. Start your favourite browser and navigate to your FQDN (i.e. zentral.example.com) 

2. The access is protected by `htaccess`  use  **test** /  **test** to get initial access

3. Congratulations, you should see the Zentral web interface now

4. We recommend to change htaccess immediately (before you go on and enroll any clients)


### gcloud session examples
As a additional reference, here's a sequence of commands in Terminal with feedback 


```bash
# list one or more accounts (we show two in the example below)
gcloud auth list                                                               
Credentialed Accounts:
 - your_name@gmail.com ACTIVE
 - name@example.org 
To set the active account, run:
    $ gcloud config set account `ACCOUNT`

# list all projects
gcloud projects list                                                           
PROJECT_ID           NAME          PROJECT_NUMBER
zentral-demo-170801  zentral-demo  82716293816  

# list all configurations
gcloud config configurations list                                                                         
NAME                 IS_ACTIVE  ACCOUNT                 PROJECT  
zentral-demo  		 True       your_name@gmail.com

# example with multiple configurations 
gcloud config configurations list                                                                         
NAME                 IS_ACTIVE  ACCOUNT                 PROJECT              
zentral-demo         True       your_name@gmail.com  zentral-demo-170801
zentral-old-demo     False      your_name@gmail.com


# create a new configuration
gcloud init                                                                    
Welcome! This command will take you through the configuration of gcloud.

Settings from your current configuration [zentral-old-demo] are:
Your active configuration is: [zentral-old-demo]

[core]
account = your_name@gmail.com
disable_usage_reporting = True

Pick configuration to use:
 [1] Re-initialize this configuration [zentral-gcloud-demo] with new settings 
 [2] Create a new configuration
Please enter your numeric choice: 2
Enter configuration name. Names start with a lower case letter and 
contain only lower case letters a-z, digits 0-9, and hyphens '-':  zentral-demo

# link the active configuration with account
Your current configuration has been set to: [zentral-demo]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.                                                                                     
Reachability Check passed.
Network diagnostic (1/1 checks) passed.

Choose the account you would like use to perform operations for this 
configuration:
 [1] your_name@gmail.com
 [2] name@example.org
 [3] Log in with a new account
Please enter your numeric choice:  1

You are logged in as: [your_name@gmail.com].
Your current project has been set to: [zentral-demo-170801].

# create and copy public zentral-all-in-one image to your project
gcloud compute images create zentral-all-in-one  --source-uri gs://zentralopensource/zentral-all-in-one.tar.gz

Created [https://www.googleapis.com/compute/v1/projects/zentral-demo-170801/global/images/zentral-all-in-one].
NAME                PROJECT              FAMILY  DEPRECATED  STATUS
zentral-all-in-one  zentral-demo-170801                      READY
>>> elapsed time 3m35s                                                                                     
# check the firewalls list - you MUST add additional ports to the defaults
gcloud compute firewall-rules list                                                                        
NAME                    NETWORK  SRC_RANGES    RULES                         SRC_TAGS  TARGET_TAGS
default-allow-icmp      default  0.0.0.0/0     icmp
default-allow-internal  default  10.128.0.0/9  tcp:0-65535,udp:0-65535,icmp
default-allow-rdp       default  0.0.0.0/0     tcp:3389
default-allow-ssh       default  0.0.0.0/0     tcp:22             

# add ports 80, 443 to the firewall rules
gcloud compute firewall-rules create default-http-https  --description "Incoming http and https allowed." --allow tcp:80,tcp:443

Created [https://www.googleapis.com/compute/v1/projects/zentral-demo-170801/global/firewalls/default-http-https].
NAME                NETWORK  SRC_RANGES  RULES           SRC_TAGS  TARGET_TAGS
default-http-https  default  0.0.0.0/0   tcp:80,tcp:443
>>> elapsed time 13s    

# list instances (to avoid billing for leftover services)
gcloud compute instances list                                                  
Listed 0 items.

# create a running instance of zentral with zentral-all-in-one image
gcloud compute instances create zentral-demo --image-project=zentral-demo --image=zentral-all-in-one
For the following instance:
 - [zentral-demo]
choose a zone:
 [1] asia-east1-a
 [2] asia-east1-b
 [3] asia-east1-c
 [4] asia-northeast1-a
 [5] asia-northeast1-b
 [6] asia-northeast1-c
 [7] europe-west1-b
 [8] europe-west1-c
 [9] europe-west1-d
 [10] us-central1-a
 [11] us-central1-b
 [12] us-central1-c
 [13] us-central1-f
 [14] us-east1-b
 [15] us-east1-c
 [16] us-east1-d
 [17] us-west1-a
 [18] us-west1-b
Please enter your numeric choice:  8

Created [https://www.googleapis.com/compute/v1/projects/zentral-demo/zones/europe-west1-c/instances/zentral-demo].
NAME          ZONE            MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP     STATUS
zentral-demo  europe-west1-c  n1-standard-2               10.132.0.2   104.199.xx.xxx  RUNNING

# connect via ssh to the image
gcloud compute ssh ubuntu@zentral-demo                                         
For the following instance:
 - [zentral-demo]
choose a zone:
 [1] asia-east1-a
 [2] asia-east1-b
 [3] asia-east1-c
 [4] asia-northeast1-a
 [5] asia-northeast1-b
 [6] asia-northeast1-c
 [7] europe-west1-b
 [8] europe-west1-c
 [9] europe-west1-d
 [10] us-central1-a
 [11] us-central1-b
 [12] us-central1-c
 [13] us-central1-f
 [14] us-east1-b
 [15] us-east1-c
 [16] us-east1-d
 [17] us-west1-a
 [18] us-west1-b
Please enter your numeric choice:  8

Updating project ssh metadata...|Updated [https://www.googleapis.com/compute/v1/projects/zentral-demo-170801].          
Updating project ssh metadata...done.                                                                                   
Warning: Permanently added 'compute.235295947160261467' (ECDSA) to the list of known hosts.

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
