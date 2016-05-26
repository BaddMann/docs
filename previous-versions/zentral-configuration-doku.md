# Zentral Configuration

## Introduction

The Zentral Configuration must be changed and customized to make [Zentral](https://github.com/zentralopensource/zentral) work for you in your own environment.

When running Zentral in a Docker setup (via docker-compose) it is mandatory to provide a custom configuration that docker containers will connect with, usually it's just a folder into your *zentral* Docker host:

```bash
/home/zentral/conf
```

To get started with Zentral we recommend to follow our new enhanced [Zentral Tutorial](https://github.com/zentralopensource/zentral/wiki/).


## Overview

The main files for your custom [Zentral](https://github.com/zentralopensource/zentral) configuration are:
>
- **base.json** - configure important App settings, provide API credentials
(SAL API key, JAMF JSS API user/password, Slack API key)
- **contacts.json** - provide the contacts for email notifications
- **Probes** - Directory with some Probes to start with, it's possible to use both JSON and YAML format.
- **tls** - A certificate, ca-cert and private key is a requirement to have in place for osquery TLS connections.
You may use the self signed cert provided here to start with, it is strongly recommend to replace these certs with your own organisation certificates.

*Important Note:*
It is a rrecommendation and requirement (enforced by osquery) to use a valid 3rd party signed SSL/TLS certificate in production - all necessary files for the client are installed on when enrollment packages from Zentral used.

---

## How to edit the configuration ?


### base.json file

The base.json is the main config file, you may want to adjust:

#### Actions

Actions get called from a Probe. You have to configure *Actions* globally in Zentral-Conf.
Actions will usually triggered once a Probe returns a result (i.e. osquery has detected a state change on a client, described with a query as part of the Probe)

 - **debug:**
 Creates files in a `/tmp/zentral_notifications/` directory on the Zentral Server (which would be your Docker host). This can useful for debugging.

```
  "debug": {
    "backend": "zentral.core.actions.backends.json_file",
    "local_dir": "/tmp/zentral_notifications/"
  },
```


 - **slack_chn:**
 Post to Slack group chat channel, you need to provide a Slack webhook to connect to the Slack API, read details to configure Slack [here](<  

```
  "slack_chn": {
     "backend": "zentral.core.actions.backends.slack",
     "webhook": "https://hooks.slack.com/services/BFBHbZFQoAnGSDDuDqWRRGUpvYnuoXAGVZtggQAh"
   },
```


 - **email:**
Post to Slack group chat channel, you need to provide a Slack webhook to connect to the Slack API,  more details to configure Slack webhooks [here](<https://api.slack.com/incoming-webhooks>)  

```
  "email": {
    "backend": "zentral.core.actions.backends.email",
    "email_from": "zentral_alerts@example.com",
    "smtp_host": "smtp.mail.example.com",
    "smtp_port": 465,
    "smtp_user": "zentral_alerts@example.com",
    "smtp_password": "<smpt_password_here>"
  },
```


#### Apps

Zentral runs specialized modules for *Inventory* and *osquery*. These modules work as Apps inside the Django Web Framework and require some custom configuration:


**Inventory:**
Configure Inventory is required to connect with your instance of **Sal** or **JAMF Software Server**


- **Example 1**  (JAMF managed clients):
Connect with a [JAMF Software Server] (<http://www.jamfsoftware.com/products/casper-suite/>) (JSS) via the JSS API (unofficial JSS API doku [here](<https://bryson3gps.wordpress.com/the-unofficial-jss-api-docs/>))

```
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

- **Example 2** (Sal dashboard for munki managed clients):
Connect with a instance of [Sal](<https://github.com/salopensource/sal>) via SalAPI.
Details to configure the [Sal API] (<https://github.com/salopensource/sal/blob/master/docs/API.md>)


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

### contacts.json file
In the contacts you store the groups you want to post notifications via email.

```
[{"first_name":"Jane",
  "last_name":"Doe",
  "email":"jane.doe@example.com",
  "phone":"0123456789",
  "groups":["admin"]
 },
 {"first_name":"John",
  "last_name":"Doe",
  "email":"john.doe@example.com",
  "phone":"9876543210",
  "groups":["nonadmin"]
 }
]
```

---

## How to create Probes ?

Probes are a vital concept of Zentral, you can create multiple *Probes* with different actions triggered on results. One probe may only push *metrics* to Prometheus, the next Probe may alert a specific group of administrators via email, while another probe will post results to a slack channel so your team can notify this. Future Actions could be open a ticket in your ticket system etc.


A Probe structure is as follows:

- **name** - A unique name for each Probe
- **osquery** - Provide a custom osquery SQL or a osquery pack
- **actions** - Actions to trigger when the probe returns a result

Probes could be created in two formats JSON and more human readable YAML


**Probes**

Each Probe is a file inside the `probes` directory:
> `../my-zentral-conf/probes`


- **Probe - JSON Example**:  
A push metrics probe

```
    {"name":"networking",
     "osquery": {
       "schedule":[
         {"query":"select interface, ipackets, opackets, ibytes, obytes from interface_details;",
          "interval":60,
          "snapshot":true,
          "key":["interface"]}
       ]
     },
     "actions":{"push_metrics": {
                  "job_name": "zentral_networking",
                  "metrics": {"ipackets": {"type":"counter",
                                           "name":"zentral_network_receive_packets",
                                           "help_text":"Received packets"},
                              "opackets": {"type":"counter",
                                         "name":"zentral_network_transmit_packets",
                                         "help_text": "Transmitted packets"},
                              "ibytes": {"type":"counter",
                                         "name":"zentral_network_receive_bytes",
                                         "help_text": "Received bytes"},
                              "obytes": {"type":"counter",
                                         "name":"zentral_network_transmit_bytes",
                                         "help_text": "Transmitted bytes"}}
                }
               }
    }
```

- **Probe - YAML Example**:  
A Probe on USB events, osquery will detect USB events and filter for specific vendor.
The configured actions in this Probe: group membership change in the JSS inventory, a post to Slack channel and *debug*.


```
name: usb
osquery:
  schedule:
    - query: select vendor, vendor_id, model, vendor in ('Rouge-Media Electronics, Inc.', 'Scary-tek') as _alert from usb_devices;
      interval: 30
  action_filter_field: _alert
  actions:
    inventory_group:
      group_name: ZENTRAL - has used blacklisted usb device
    slack_chn1: null
    debug:
      sub_dir: usb
```

*Note: To see this probe in action look a demo video [here](https://youtu.be/hdDoWK0A9TQ)*
