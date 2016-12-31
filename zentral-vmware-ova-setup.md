# Zentral - VMware OVA  

https://www.vmware.com/support/developer/ovf/ovf420/ovftool-420-userguide.pdf
The Vagrant / VirtualBox based setup is quick way to start Zentral.

## Intro
This document will help you to start [Zentral](https://github.com/zentralopensource) on **VirtualBox with Vagrant**. 

For evaluation we use [Vagrant](https://www.vagrantup.com/docs/) to launch a local instance from a pre-build vagrant-box with just a few commands. The time to download the pre-build vagrant-box from Hasicorp Atlas may take a while (approx 15-25min depending on internet connection).

The box is publically hosted here: <https://atlas.hashicorp.com/zentral/boxes/all-in-one>

*Note: We also provide a guide for a Cloud based setups - please look [here](https://github.com/zentralopensource/docs/blob/master/zentral-gcloud-setup.md) for Google Cloud and [here](<https://github.com/zentralopensource/docs/blob/master/zentral-aws-setup.md>) for Amazon AWS setup.*

## Overview

This is a guide to run a local instance [Zentral](https://github.com/zentralopensource/zentral) deployed with **Vagrant on VirtualBox**. 
We show how to launch a instance of Zentral based on our publicly available **Zentral all in one** pre-build box. The pre-build image is build with Packer, Ansible is used as provisioner. 

1. It's useful to have the Vagrant documentation at hand, please start to look into the [Vagrant getting started guide](https://www.vagrantup.com/docs/getting-started/).

2. Read the requirements - you need to have latest VirtualBox and Vagrant installed. 

3. We provide a sequence of steps in the Vagrant section and launch a pre-build "Zentral all in one" vagrant box locally. 

4. Read the  overview of **Zentral all in one** build in CLI tools. These are available in `/home/zentral/app/utils/` once you connect via `vagrant ssh` to the running instance.

5. Please also read section about configuration files in Zentral. We  recommend to change the default htaccess protection in `zentral.htpasswd` file, and you may want to enable integrations for 3rd party services in the `base.json` file.

*Note: Please keep in mind this tutorial is a starting point, a full walk thru all options of Vagrant, VirtualBox, etc. is obviously out scope.*

## Requirements:

You need to have the following already in place to start deployment of **Zentral all in one**  with Vagrant. 

- A current version of VirtualBox (start here: <https://www.virtualbox.org/wiki/Downloads> - at time of writing version is 5.1.12.)
- A current version of Vagrant installed (start here: <https://www.vagrantup.com/downloads.html>)
- The regular vagrant workflows and commands apply

## Vagrant / VirtualBox setup

We have some example screenshots for Vagrant based setup [here](https://github.com/zentralopensource/docs/blob/master/images_vagrant), you can follow them along this document.

### Vagrant Commands

https://www.vmware.com/support/developer/ovf/ovf420/ovftool-420-userguide.pdf


`vagrant init zentral/all-in-one`  

```bash
/Applications/VMware\ Fusion.app/Contents/Library/VMware\ OVF\ Tool/ovftool --datastore=VM_Datastore /Users/admin/Documents/Virtual\ Machines.localized/zentral-all-in-one.ova vi://root:<password>@192.168.xx.xx
Opening OVA source: /Users/admin/Documents/Virtual Machines.localized/zentral-all-in-one.ova
The manifest validates
Opening VI target: vi://root@192.168.xx.xx:443/
Deploying to VI: vi://root@192.168.xx.xx:443/
Transfer Completed                    
Completed successfully
>>> elapsed time 12m39s  
```

`vagrant up --provider virtualbox`

```bash
vagrant up --provider virtualbox                                               

Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'zentral/all-in-one'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'zentral/all-in-one' is up to date...
==> default: Setting the name of the VM: zentral_vagrant_default_1482837994005_62560
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => /Users/admin/zentral_vagrant
>>> elapsed time 1m30s                 
```

**SSH into your instance**

With Vagrant you simply need to type `vagrant ssh` to connect via ssh.

```bash
vagrant ssh                                                                                          
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-57-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.


----------------------------------------------------------------
  Ubuntu 16.04.1 LTS                          built 2016-12-24
----------------------------------------------------------------
```

**Setup IP in /etc/hosts**

While connected to the box via ssh use the `ifconfig` command to get the IP for the instance.

```bash
vagrant@vagrant:~$ ifconfig
enp0s3    Link encap:Ethernet  HWaddr 08:00:27:c3:24:ad  
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fec3:24ad/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:910 errors:0 dropped:0 overruns:0 frame:0
          TX packets:726 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:92026 (92.0 KB)  TX bytes:86776 (86.7 KB)

enp0s8    Link encap:Ethernet  HWaddr 08:00:27:58:c5:5b  
          inet addr:172.28.128.3  Bcast:172.28.128.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe58:c55b/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2 errors:0 dropped:0 overruns:0 frame:0
          TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:1180 (1.1 KB)  TX bytes:1332 (1.3 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:2189 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2189 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:740653 (740.6 KB)  TX bytes:740653 (740.6 KB)
```

`sudo nano /etc/hosts` 

Add IP from the Vagrant box, with entry for zentral

**Setup  /etc/hosts **

```bash
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
172.28.128.3    zentral
```


**Setup a self signed TLS cert for your instance**

1. Make sure you've set IP address and a hostname in the `/etc/hosts` file

2. Use the following command, provide the hostname from etc/hosts as parameter
 `sudo /home/zentral/app/utils/set_fqdn.py --self-signed-cert zentral`

3. You'll see feedback in Terminal about a unique **dhparam** and self signed
certificate created.

```bash
vagrant@vagrant:~$ sudo /home/zentral/app/utils/set_fqdn.py --self-signed-cert zentral
Generating a 2048 bit RSA private key
............................................+++
.........................................................+++
writing new private key to '/etc/nginx/ssl/zentral.key'
-----
Generate new dhparam ...
Generating DSA parameters, 4096 bit long prime
.+.............+......+....+.................+..............+...+...........+...+..................................................+..................+..+.............+.......+..+...............+................+..+...........+++++++++++++++++++++++++++++++++++++++++++++++++++*....+.+...............+...........+.........+..............+...+++++++++++++++++++++++++++++++++++++++++++++++++++*
New dhparam generated
```




**Open Zentral in your Browser**

1. Start your favourite browser and navigate to your local instance (i.e. https://zentral) 

2. The access is protected by `htaccess`  use  **test** /  **test** to get initial access

3. Congratulations, you should see the Zentral web interface now

4. We recommend to change htaccess 


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

## Edit Zentral configuration

Although this version of zentral is running locally via Vagrant you may also try to add or change `zentral` configuration. For changes you need to ssh into your instance via `vagrant ssh`, change into the directory with `cd /home/zentral/conf`. You may edit the following files here:

- `base.json` - edit here for the main configuration like adding actions, integrations, etc. 
- `contacts.json` edit here for contacts of admins, admin-groups to notify via email, SMS
- `zentral.htpasswd` edit here for change the password htaccess protected Zentral web interface, Kibana, Prometheus


The Zentral configuration options are also used to enable integration with 3rd party tools. A simple addition to the `base.json` configuration file will enable actions/notifications. This will enable custom actions for Probes to trigger notifications to Slack, add cards to a Trello board, create a ticket in Zendesk, etc.
Providing URL and tokens in the `base.json` configuration file is required to connect the *Zentral Inventory worker* via REST API and fetch inventory data and/or change group membership in JAMF Pro, Watchman Monitoring, and Sal.
