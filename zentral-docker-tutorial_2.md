## Episode 2 - Create a custom `my-zentral-conf`
Zentral needs to have a configuration to fully operate, we will download a demo-config and turn this into your own `my-zentral-config` and on the docker host.

*Note:* 
To check out Slack integration you must have enabled webhooks in the Slack API. How to setup a Slack channel and the Slack API is beyond the scope of this tutorial, best have a look [here](<https://api.slack.com/incoming-webhooks>)

#### Clone Zentral demo-conf, turn into your `my-zentral-confg`


1.) Download the [Zentral demo-conf](<https://github.com/zentralopensource/zentral-conf>) from GitHub: 

**Method A** - Use a Web browser, click on this link: <https://github.com/zentralopensource/zentral-conf/archive/master.zip>

**Method B** - Use `git` and clone the files with this command: 
`git clone https://github.com/zentralopensource/zentral-conf.git` 

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

