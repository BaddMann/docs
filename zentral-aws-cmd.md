## Working with the Zentral configuration and setup

In **AWS** based setup:

To change the `zentral` configuration, edit files in `/home/zentral/conf` on the server. Mainly you may edit the following:

- `base.json` - edit here for the main configuration like adding actions, integrations, etc. 
- `contacts.json` edit here for contacts of admins, admin-groups to notify via email, SMS
- `/etc/nginx/zentral.htpasswd` edit here for change the password htaccess protected Zentral web interface, Kibana, Prometheus


## Commands for AWS ubuntu AMI 'Zentral all in one' (search in Public images)

To bootstrap with FQDN:
Requirements:

- aws ssh-key must be set to connect to the AMI
- username to connect with is "ubuntu" (AWS default ubuntu)
- AWS AMI assigned IP address must be set in your DNS (so Let's Encrypt will work)
- ports in security group must be opened for 443, 80, 22

```bash
sh -i .ssh/ssh-key-zentral-aws.pem ubuntu@zentral.example.org sudo /home/zentral/app/utils/set_fqdn.py zentral.example.org

```

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
