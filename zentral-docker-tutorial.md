![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)

# Zentral Docker Tutorial

## Overview
[Zentral](https://github.com/zentralopensource/zentral) is a new tool in the *#macadmins* ecosystem it is in heavy development so expect changes and additions in the future.
To evaluate, learn and quickly deploy Zentral you should run [Docker](<https://docs.docker.com>) and start with the `Zentral Docker image` posted on Docker Hub: <https://hub.docker.com/r/zentral/zentral/>

This Tutorial will guide you to:

- Pull the `zentral/zentral` image from Docker Hub.
- Create a custom `my-zentral-config` Zentral configuration.
- Run Zentral in Docker, check your `my-zentral-config`
- Run Zentral in Docker, in extended Debugging mode
- Enroll a OSX device in Terminal from the command line
- Enter the Docker host for Debugging.

---

## Requirements

- You need to have a TextEditor (i.e. use the free of charge [Atom](<https://atom.io>) Editor from GitHub) 
- You need to run commands in the Terminal 
- You need to have a Docker host running
- You need to have osquery v1.5.3 (or later) installed on a OS X client device
- Your OS X client must match osquery system requirements
 

## Prepare your Docker host
 
### Do you already have a Docker host up and running ?
It's recommended to clean up your Docker playground before starting this Tutorial.
 
 - Install latest Docker 1.8.3, [more info] (<https://blog.docker.com/category/engineering/docker-releases/>)
 - Ensure your Docker host is currently not running a container on port 443
 - Ensure you have enough space 
 [TIP: use these docker commands to check your Docker host state: 
 `docker ps`,  `docker ps-a`, `docker images`] 
 
### You don't have a Docker host setup running ? 
Here is a mini instruction to set one up for you. 


 1.) Install Ubuntu 14.04 LTS (server or client both work) in [VMware Fusion](< or [VirtualBox](<https://www.virtualbox.org/wiki/Downloads>)
 2.) Follow this Docker for Linux: <https://docs.docker.com/linux/step_one/>
 
 *Note*: 
 We recommend to run Docker on Linux and avoid OS X as Docker host (as it requires VirtualBox anyways) 
 
 
##### tl;dr
 ```bash
# ssh into Ubuntu 14.04LTS, run command below to install latest docker
 curl -sSL https://get.docker.com | sh
 ```
 
 ---
 
## Episode 1 - Start to install Zentral in Docker

You need to have some Terminal skills to get started with Docker. 
We will pull the `zentral/zentral` Docker image from Docker Hub in this Episode.

#### Pull the `zentral/zentral` image from Docker Hub

1.) Find out the IP of your Docker host
2.) Use Terminal to connect with `ssh` to your Docker host

```bash
## NOTE: your IP will be different to the one we use below.
## connect as user root to a Docker host with IP 192.168.56.10

ssh root@192.168.56.10
```

3.) Run a short Docker version check in Terminal 
Use the command: `docker --version`

```bash
  root@dockerhost:~$ docker --version
  Docker version 1.8.3, build f4bf5c7
```

4.) Pull the zentral/zentral Docker image from Docker Hub, this process will take a while (5-20min depending on your d/l speed)
Use the command: `docker pull zentral/zentral`

```bash
root@dockerhost:~$ docker pull zentral/zentral
Using default tag: latest
latest: Pulling from zentral/zentral

3fd0c2ae8ed2: Pulling fs layer 
9e19ac89d27c: Pulling fs layer 
ac65c371c3a5: Pulling fs layer
... 
... # snipped out lengthy strings here
...
4d5ef7a0612a: Pull complete 
aefa3f7e9fea: Pull complete 
Digest: sha256:08c26f427cf04dbc95e30c885f1d6c138c469b40962763b21fc215e43cb57e78
Status: Downloaded newer image for zentral/zentral:latest
```

### Expected Result: 
You have managed to download `zentral/zetral` image from Docker Hub with the `docker pull` command.
 
--- 

## Episode 2 - Create a custom `my-zentral-config`
Zentral needs to have a configuration to fully operate, we will download a demo-config and turn this into your own `my-zentral-config` and on the docker host.

*Note:* 
To check out Slack integration you must have enabled webhooks in the Slack API. How to setup a Slack channel and the Slack API is beyond the scope of this tutorial, best have a look [here](<https://api.slack.com/incoming-webhooks>)

#### Clone Zentral demo-conf, turn into your `my-zentral-config`


1.) Download the [Zentral demo-conf](<https://github.com/zentralopensource/zentral-conf>) from GitHub: 
    **Method A** - Use a Web browser, click on this link: <https://github.com/zentralopensource/zentral-conf/archive/master.zip>
    **Method B** - Use `git` and clone the files with this command: `git clone https://github.com/zentralopensource/zentral-conf.git` 
    
```bash
  root@dockerhost:~$ git clone https://github.com/zentralopensource/zentral-conf.git
  Cloning into 'zentral-conf'...
  remote: Counting objects: 18, done.
  remote: Compressing objects: 100% (17/17), done.
  remote: Total 18 (delta 1), reused 18 (delta 1), pack-reused 0
  Unpacking objects: 100% (18/18), done.
  Checking connectivity... done.

``` 
2.) Open the **base.json** file in your preferred text editor (vi, nano, Atom.app)
3.) Change the following areas to match with your environment

- Edit the **kibana_base_url** - you must provide a URL that can resolve to your docker host.

```bash

    "elasticsearch": {
      "backend": "zentral.core.stores.backends.elasticsearch",
      "servers": ["http://localhost:9200"],
      "index": "zentral-events",
      "kibana_base_url": "https://zentral.example.com/kibana/"
    }
```

- Edit the **webhook** - you must provide valid `Webhook URL` for posting to Slack. 


```bash
"actions": {

    "slack_channel": {
      "backend": "zentral.core.actions.backends.slack",
      "webhook": "https://hooks.slack.com/services/AUkeEbkoksOWkIKdESfgpqJVbQILXkXShQjQobmR"
    },
},
```

- Look up the **enroll_secret_secret** - you must use the exact the same String combination on the clients.

```bash
"apps": {
   "zentral.contrib.osquery": {
     "enroll_secret_secret": "FOOBARBAZ"
   }
```
*Note*:
You may want to change this in Production but for the purpose of this tutorial you can leave it untouched.


4.) Connect to Inventory
We currently support Sal API (receiving Inventory for munki managed clients) and JAMF JSS API (receiving Inventory for JAMF managed clients).


**Option A: JAMF Inventory Support**

a) In your JSS create a specific user and enable API read access.
b) You may consider `write` access as well in case you want to use Zentral to change group membership in JSS as an action.


- Edit the **host** - you must provide valid `JSS URL`. 
- Edit the **port** - you must provide a valid `port` for your JSS (JAMF default is 8443). 
- Edit the **user** - you must provide a JSS user with permissions to use the JSS API.
- Edit the **password** - you must provide a password to connect with the JSS API.

```bash
"apps": {
   "zentral.contrib.inventory": {
     "backend": "zentral.contrib.inventory.backends.jss",
     "host": "jss.example.com",
     "port": "8443",
     "path": "/JSSResource",
     "user": "API_user",
     "password": "API_password"
   },
```
*Interesting Side Note*: 
The `inventory_group` action in `base.json` can change membership in JSS groups
We will go into details in a JSS focused Zentral tutorial (posted soon).

```bash

"actions": {

    "inventory_group": {
      "backend": "zentral.contrib.inventory.actions.add_machine_to_group"
    }
},
```


**Option B: Sal Inventory Support**

a) In Sal go to Settings and create a new API key <http://sal.example.com/settings/api-keys/new/>
b) Note both `public_key` and `private_key` carefully to copy/paste into your config

- Edit the **host** - you must provide valid `URL` to your Sal instance. 
- Edit the **secure** - you must provide `false` in case you run a non SSL based Sal (i.e. Sal Docker instance)
- Edit the **public_key** - you must provide valid `public_key`
- Edit the **private_key** - you must provide valid `private_key` 

```
"apps": {
 "zentral.contrib.inventory": {
   "backend": "zentral.contrib.inventory.backends.sal",
   "host": "sal.example.com",
   "secure": false,              
   "public_key": "aDiQTFwpWDWOzrrunCLTkmAp",
   "private_key": "kaUHozVUQtptuxCOMEjQrwqDndZUDPVKEBVQNhOrHUEPCKZoTPzppndeDCPgYSvC"                 
 },
```

*Note*:
Currently the SAL API group changes are technically possible but would not match with common style Sal is used.
We already talk to **Graham Gilbert** on future ideas to integrate with the Sal API.



4.) Upload or move your edited demo config to the Docker host.
It's important that you copy the full directory including the subdirectories.

- If you have edited the demo-config on your Docker host:

```bash
## copy all files to the /opt directory
cp -a ../zentral-conf /opt/my-zentral-conf

```

- If you have edited the demo-config on another device:

```bash
## pseudo code here, adjust for your envirionment
scp -r zentral-conf your_username@dockerhost:/opt/my-zentral-conf
```


*Note*: You may reach out for GUI like Transmit to copy the files via SFTP


5.) Verify edited config files are in `/opt` directory on  your Docker host
 Use the command: `ls -l /opt/my-zentral-conf`

```bash
 root@dockerhost:~$ ls -l /opt/my-zentral-conf/
 total 20
 -rw-r--r-- 1 root root 1750 Oct 30 09:10 base.json
 -rw-r--r-- 1 root root  258 Oct 30 09:10 contacts.json
 drwxr-xr-x 2 root root 4096 Oct 30 09:10 elasticsearch
 drwxr-xr-x 2 root root 4096 Oct 30 09:10 probes
 drwxr-xr-x 2 root root 4096 Oct 30 09:10 tls
```



### Expected Result: 
You have created a custom `my-zentral-conf` and moved it to `/opt` directory on your docker host.

---

## Episode 3 - Run Zentral in Docker, check your `my-zentral-conf`
To ensure your custom config is working we will use a check procedure

1) Use the Terminal connected to the docker host.
Use the command: docker run -t -i -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral check


- If Check Zentral configuration is running fine you should see the following result. 
No result means your config should be OK, aka the check should not return any error.

```bash
 root@dockerhost:~$ docker run -t -i -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral check

Check Zentral configuration

```


- If you have an error in your Zentral configuration you may see the following result.
Common errors are usually wrong terminated JSON structs - read more on JSON Structs [here](<http://www.w3resource.com/JSON/introduction.php>)

```bash
root@dockerhost:~$  docker run -t -i -v /opt/my-znetral-conf:/home/zentral/conf zentral/zentral check

Check Zentral configuration

Traceback (most recent call last):
  File "/home/zentral/zentral/bin/check_configuration.py", line 6, in <module>
    zentral.setup()
  File "/home/zentral/zentral/__init__.py", line 9, in setup
    from zentral.conf import settings
  File "/home/zentral/zentral/conf/__init__.py", line 65, in <module>
    settings = load_config_file(find_conf_file(conf_dir, "base"))
  File "/home/zentral/zentral/conf/__init__.py", line 60, in load_config_file
    raise ImproperlyConfigured("{} error in file {}".format(filetype, filepath)) from None
zentral.core.exceptions.ImproperlyConfigured: JSON error in file /home/zentral/conf/base.json

```
*NOTE*: 
You may also try a JSON linter but be extremely careful to post your `base.json` to any online JSON linter - remember you have just inserted some sensible data a moment ago that should not go public in any way (we've warned you here and take no responsibility).


### Expected Result: 
You have successfully run Zentral in Docker(just for a second of course), started a short check for your config and are ready to go on next. 

TIP: As an optional exercise you could have changed the contents of the `tls` folder - we provide some self signed TLS certs but for production you should create your own and best get a 3rd party signed SSL/TLS cert from <https://letsencrypt.org>  (soon) or get a commercial one from [COMODO](<https://ssl.comodo.com/comodo-ssl-certificate.php>) or [VeriSign/Symantec](<https://www.symantec.com/ssl-certificates/>)



---

## Episode 4 - Run Zentral in Docker, in extended Debugging mode
Now we are ready to run Zentral in Docker and link our `my-zentral-config`.
But we don't stop here, we run `docker` with additional options to learn and debug in this tutorial.

1) We start docker with default SSL/TLS Port 443 with the `-p` flag, we link directories on the Docker host with the `-v` flag 
Use this command:
`docker run -t -i -p 443:443 -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral`

2) Start a WebBrowser and enter to your Docker host FQDN or IP address as URL with `https://<yourFQDNhere>/inventory` <https://zentral.excample.com/inventory/> 


3) To enable more options we can run `docker` with additional flags (useful for debugging and inspecting the zentral docker container). 
Use this command:
`docker run -t -i -p 443:443 -v /tmp/supervisorlog:/var/log/supervisor -v /tmp/zentral_notifications:/tmp/zentral_notifications -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral`


```bash
## the result starting docker image should look like this for you
root@dockerhost:~$  docker run -t -i -p 443:443 -v /tmp/supervisorlog:/var/log/supervisor -v /tmp/zentral_notifications:/tmp/zentral_notifications -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral

Check Zentral configuration

Launch supervisor

2015-10-30 10:17:44,534 CRIT Supervisor running as root (no user in config file)
2015-10-30 10:17:44,566 INFO RPC interface 'supervisor' initialized
2015-10-30 10:17:44,566 CRIT Server 'unix_http_server' running without any HTTP authentication checking
2015-10-30 10:17:44,567 INFO supervisord started with pid 7
2015-10-30 10:17:45,572 INFO spawned: 'postgres' with pid 10
2015-10-30 10:17:45,575 INFO spawned: 'server_gunicorn' with pid 11
2015-10-30 10:17:45,579 INFO spawned: 'zentral_store_event_worker' with pid 12
2015-10-30 10:17:45,583 INFO spawned: 'redis' with pid 13
2015-10-30 10:17:45,621 INFO spawned: 'kibana' with pid 16
2015-10-30 10:17:45,626 INFO spawned: 'nginx' with pid 17
2015-10-30 10:17:45,637 INFO spawned: 'prometheus' with pid 18
2015-10-30 10:17:45,655 INFO spawned: 'elasticsearch' with pid 23
2015-10-30 10:17:45,682 INFO spawned: 'zentral_process_event_worker' with pid 25
2015-10-30 10:17:45,715 INFO spawned: 'zentral_inventory_worker' with pid 30
2015-10-30 10:17:45,729 INFO spawned: 'prometheus_push_gateway' with pid 31
2015-10-30 10:17:47,143 INFO success: postgres entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: server_gunicorn entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: zentral_store_event_worker entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: redis entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: kibana entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: nginx entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: prometheus entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: elasticsearch entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: zentral_process_event_worker entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: zentral_inventory_worker entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: prometheus_push_gateway entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)

```

*Note*:
We run the later option 3) as our default here.


### Expected Result: 

You should see the Zentral web interface successfully display your inventory. The key to success here is you must have provided the right API settings to enable Zentral.core connect to `Sal` or your `JSS` Server within the **base.json** .


---

## Episode 5 - Enroll a OSX device in Terminal from the command line
Of course Zentral needs osquery client to enroll and connect with, this is what we're going to do now.
For Production you will use a LaunchDaemon on all your devices starting osqueryd, but for this tutorial we're fine with
a `--verbose`session in Terminal.


1.) In your DNS settings do not resolve your docker host you may want to edit your `/etc/hosts` file on your admin and on test client machine

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
192.168.94.134  zentral zentral.example.com
```


2.) Check you have osquery installed, open a Terminal window on your OS X client device. 
Use this command:
`osqueryi --version`

```bash
  ## use the shortcut to check for osquery version
  osqueryi --version
  osqueryi version 1.5.3
```

*Note*: 
You may want to download latest osquery .pkg installer here: <https://osquery.io/downloads/>

3.) Copy the `zentral.crt` from the [Zentral-demo-conf] (<https://github.com/zentralopensource/zentral-conf/archive/master.zip>) `tls` directory to your OS X client, for our Tutorial we expect the file in `/Users/Shared` directory.

4.) We need to create a `--database_path` directory for osquery files and it's `RocksDB` database.

```bash
  ## use the shortcut to check for osquery version
  mkdir -p /tmp/zentral/osquery
```
5.) We must create a `enroll_secret.txt` file to make a successful connection from `osqueryd` to the TLS **Zentral/osquery** App.
The osquery `enroll_secret.txt`consist of a shared secret represented in the **base.json** file
We need to bild a simple text file with these ingredients `Shared Secret` and `Computer Serial`

then final file should just contain a strin in this format

```bash
## Pseudo code example
  <your_enroll_secret_here>$SERIAL$<your_device_hardware_serial_here>
```





- Check for your computer serial number

```bash
  /usr/sbin/system_profiler SPHardwareDataType | awk '/Serial/ {print $4}'
``` 

- Create a text file in `/Users/Shared/`

```bash
  touch /Users/Shared/enroll_secret.txt
```

```bash
  ## use your own machine serial in this command
  echo "FOOBARBAZ$SERIAL$C02Q239GHG2" > /Users/Shared/enroll_secret.txt
```

- Verify the text file `/Users/Shared/enroll_secret.txt`

```bash
  cat /Users/Shared/enroll_secret.txt

  FOOBARBAZ$SERIAL$C02Q239GHG2
```

 
6.) We start the `osqueryd` binary with a lot of arguments, you must change at least one to make it work successfully

- **--tls_hostname** - you must insert your FQDN or IP from the docker host here.

```bash
## this command must be run with sudo, you will need to provide your password 
sudo osqueryd --database_path=/tmp/zentral/osquery --verbose --config_plugin=tls --tls_hostname=zentral.example.com --tls_server_certs=/Users/Shared/zentral.crt --config_tls_endpoint=/osquery/config --config_tls_refresh=60 --enroll_tls_endpoint=/osquery/enroll --enroll_secret_path=/Users/Shared/enroll_secret.txt --logger_tls_endpoint=/osquery/log --logger_tls_period=31 --logger_plugin=tls --distributed_enabled --distributed_plugin=tls --distributed_tls_read_endpoint=/osquery/distributed/read --distributed_tls_write_endpoint=/osquery/distributed/write --distributed_poll_interval=60

```

- The **--verbose** flag should result with output in your Terminal window equal to this example:

```bash
## example 
sudo osqueryd --database_path=/tmp/zentral/osquery --verbose --config_plugin=tls --tls_hostname=zentral.example.com --tls_server_certs=/Users/Shared/zentral.crt --config_tls_endpoint=/osquery/config --config_tls_refresh=60 --enroll_tls_endpoint=/osquery/enroll --enroll_secret_path=/Users/Shared/enroll_secret.txt --logger_tls_endpoint=/osquery/log --logger_tls_period=31 --logger_plugin=tls --distributed_enabled --distributed_plugin=tls --distributed_tls_read_endpoint=/osquery/distributed/read --distributed_tls_write_endpoint=/osquery/distributed/write --distributed_poll_interval=60
I1030 19:01:07.904667 1957675008 init.cpp:273] osquery initialized [version=1.5.3]
I1030 19:01:07.929126 1957675008 processes.cpp:183] An error occurred retrieving the env for pid: 84437
I1030 19:01:07.929256 1957675008 system.cpp:172] Found stale process for osqueryd (84437) removing pidfile
I1030 19:01:07.929486 1957675008 system.cpp:207] Writing osqueryd pid (84476) to /var/osquery/osqueryd.pidfile
I1030 19:01:07.929826 1957675008 extensions.cpp:171] Could not autoload extensions: Failed reading: /etc/osquery/extensions.load
I1030 19:01:07.930681 528384 watcher.cpp:366] osqueryd watcher (84476) executing worker (84477)
I1030 19:01:07.942600 1957675008 init.cpp:270] osquery worker initialized [watcher=84477]
I1030 19:01:07.943572 1957675008 extensions.cpp:178] Could not autoload modules: Failed reading: /etc/osquery/modules.load
I1030 19:01:07.943727 1957675008 db_handle.cpp:124] Opening RocksDB handle: /tmp/zentral/osquery
I1030 19:01:07.950057 1601536 interface.cpp:220] Extension manager service starting: /var/osquery/osquery.em
I1030 19:01:07.950099 1957675008 db_handle.cpp:124] Opening RocksDB handle: /tmp/zentral/osquery
I1030 19:01:07.953806 1957675008 tls.cpp:68] TLSEnrollPlugin requesting a node enroll key from: https://zentral.example.com/osquery/enroll
I1030 19:01:07.954454 1957675008 tls.cpp:196] TLS/HTTPS POST request to URI: https://zentral.example.com/osquery/enroll
I1030 19:01:08.181259 1957675008 tls.cpp:196] TLS/HTTPS POST request to URI: https://zentral.example.com/osquery/config
I1030 19:01:08.426236 1957675008 events.cpp:564] Event publisher failed setup: kernel: Cannot access /dev/osquery
I1030 19:01:08.426491 1957675008 file_access_events.cpp:45] Added kernel listener to: /Users/Shared/
I1030 19:01:08.426697 1957675008 file_events.cpp:58] Added listener to: /Users/Shared/**
I1030 19:01:08.427011 7454720 events.cpp:507] Starting event publisher run loop: diskarbitration
I1030 19:01:08.427014 7991296 events.cpp:507] Starting event publisher run loop: fsevents
I1030 19:01:08.427034 8527872 events.cpp:507] Starting event publisher run loop: iokit_hid
I1030 19:01:08.427048 9064448 events.cpp:507] Starting event publisher run loop: scnetwork
I1030 19:01:08.427284 9601024 tls.cpp:196] TLS/HTTPS POST request to URI: https://zentral.example.com/osquery/distributed/read
```

7.) Wait for a short moment then go to the Zentral > Inventory section and look into the events returned to Zentral: 


- As you know your machine serial can directly enter the events by using a URL like this: 
<https://zentral.example.com/inventory/machine/C02Q239GHG2/events/>


*Note*: 
Kibana and Prometheus are installed with Zentral - we will cover those in another tutorial later.

- To inspect Prometheus the URL would be like: <https://zentral.example.com/prometheus/>
- Kibana requires to **Configure an index pattern** for first use, the URL would be: <https://zentral.example.com/kibana/>




### Expected Result: 
Your Client should enroll with the **Zentral/osquery** App running inside the `zentral/zentral` container on your docker host.
The Probes will download to the client and processed, you will see frequently in terminal how the `osqueryd` binary will contact Zentral via TLS, runs the SQL query and will post back results to Zentral, if Actions trigger in the Probes you should see the in the Zentral User Interface `Inventory > Machine Serial > View Events` Sectio.n


---

## Episode 6 - Enter the Docker host for Debugging.

In case things don't work as expected you may want to find the bug.
So here is a short but (hopefully) usefull debugging session to help you with some examples and logfiles to look into.

#### Docker host debug with logfiles:

1) In a new Terminal connect to your docker host, we want look into the debugging options now.
You may find errors in these areas:

- **Performance** or startup errors - check the:  `server_gunicorn-stderr---supervisor--XXXXX.log` file
- **inventory** connection problems - check the:  `zentral_inventory_worker-stderr---supervisor-XXXXX.log` file
- **Action** or processing errors - check the:  `zentral_process_event_worker-stderr---supervisor-XXXXX.log` file
- **TLS** or Nginx Web server errors - check the:  `nginx-stderr---supervisor-XXXXX.log` file


```bash
root@dockerhost:~$ ls supervisorlog/
elasticsearch-stderr---supervisor-9p9VYn.log
elasticsearch-stdout---supervisor-WqLMIF.log
kibana-stderr---supervisor-pY_lZ2.log
kibana-stdout---supervisor-Xwltxo.log
nginx-stderr---supervisor-YFF5JY.log
nginx-stdout---supervisor-cppOu4.log
postgres-stderr---supervisor-qxyqfN.log
postgres-stdout---supervisor-Mcbb3U.log
prometheus-stderr---supervisor-nZi0m5.log
prometheus-stdout---supervisor-jCxJt2.log
prometheus_push_gateway-stderr---supervisor-f6BaVt.log
prometheus_push_gateway-stdout---supervisor-Cn5gYt.log
redis-stderr---supervisor-77SVQa.log
redis-stdout---supervisor-rE0JAR.log
server_gunicorn-stderr---supervisor-OqX1RS.log
server_gunicorn-stdout---supervisor-PHLrxz.log
supervisord.log
zentral_inventory_worker-stderr---supervisor-KArl02.log
zentral_inventory_worker-stdout---supervisor-ALsEUe.log
zentral_process_event_worker-stderr---supervisor-n6DeOH.log
zentral_process_event_worker-stdout---supervisor-6E5qEO.log
zentral_store_event_worker-stderr---supervisor-YJOi61.log
zentral_store_event_worker-stdout---supervisor-d36tPm.log
```

2) The next debugging technique is one targeted onActions, by default in base.json you'll find `debug` option setting activated.
Once an Probe `Action` triggers a timestamped JSON file will be written to the `local_dir`.


```bash
  "actions": {
    "debug": {
      "backend": "zentral.core.actions.backends.json_file",
      "local_dir": "/tmp/zentral_notifications/"
    },
```

#### Example debug case:
You have setup a Slack webhook in your **base.json** but no info happens to be posted to the Slack channel, how to find out if at least the Probe Actions were successfull?

Connect tom the Docker host and consult the `local_dir` (defaults to `/tmp/zentral_notifications/`). 
See if you find some matching subdirectories and time-stampted files inside. 
Look inside the files (using the `cat` unix tool), if so your Probes may work fine but the Slack API/webhook should be double checked (with `curl` like provided in the [Slack API doku](<https://api.slack.com/incoming-webhooks>) ). 


- Empty debug directory looks like this:

```bash
root@dockerhost:~$ ls -la /tmp/zentral_notifications/
total 8
drwxr-xr-x 2 root root 4096 Oct 30 11:15 .
drwxrwxrwt 9 root root 4096 Oct 30 11:42 ..
```

- Inspect a the debug directory like this:

```bash
root@dockerhost:~$ ls -la /tmp/zentral_notifications/
drwxr-xr-x 5 root root 4096 Oct 30 12:01 .
drwxrwxrwt 9 root root 4096 Oct 30 11:42 ..
drwxr-xr-x 2 root root 4096 Oct 30 12:01 dropbox_alert
drwxr-xr-x 2 root root 4096 Oct 30 12:01 little_snitch
drwxr-xr-x 2 root root 4096 Oct 30 12:01 osx-attacks

root@dockerhost:~$ ls -la /tmp/zentral_notifications/dropbox_alert/
total 12
drwxr-xr-x 2 root root 4096 Oct 30 12:01 .
drwxr-xr-x 6 root root 4096 Oct 30 12:02 ..
-rw-r--r-- 1 root root  429 Oct 30 12:01 2015-10-30T11:01:05.986639

root@dockerhost:~$ cat /tmp/zentral_notifications/dropbox_alert/2015-10-30T11\:01\:05.986639 
{
    "body": "Name: mb-sal\nSerial number: C02Q239GHG2\n\nModel: Apple, MacBook (12-inch Retina Early 2015)\nProcessor: Intel Core M 1.2GHZ\nTotal RAM: 8.0 GiB\nFileVault 2: not encrypted\nView in jss: https://jss.example.com:8443/computers.html?id=52&o=r\nZentral event id: 0538753a-2b99-466c-b20f-6c31d5cf9a66\n\naction: added\nname: Dropbox\npid: 586\nport: 17500\n",
    "subject": "Zentral notification - dropbox_alert"

```

### Expected Result: 
You should see debug files for Notifications in `/tmp/zentral_notifications/` directory. In case of errors you know the logfiles to look into and provide them as feedback for bug reports.


---

## Wrap Up


This was a full tutorial on Docker and Zentral. Thanks for taking the time to read thru and try out Zentral in Docker.
We hope it's even more fun to learn osquery and come up with cool tricks using Zentral in your environment.
Of course we could not covered all details and options with docker and would love to see contributions, submit errata and comments for improvements.

Few areas we'd love to get specific contributions:

- Tutorial running zentral/zentral docker image behind a reverse proxy docker to terminate TLS 
( Yes we know this project <https://github.com/jwilder/nginx-proxy> but did not have much time to play with )

- Tutorial for Kibana data visualization. 

- Probes to share with the community. 
(We hope to learn more from Greg Neagle and Tim Sutton how they handle the community submissions for AutoPKG)

- Code contribution - start to look into the [Zentral source code](https://github.com/zentralopensource/zentral). 
Create and contribute additional Actions (Hint: Slack is cool, but what about HipChat?).


## Thank You

A special thanks goes to: 
Allister Banks, Marko Jung, Tim Sutton, and Arek Dryer for kicking the wheel during MacSysAdmin 2015. To Graham Gilbert for help with Sal API, Teddy Reed and Mike Arpaia from Facebook/osquery for their amazing work and last but not least to *you* for taking interest in Zentral.

---
