# Reference - Zentral tutorial

This is a short command reference for the Zentral - Info Overview and deep dive [video tutorial](<https://www.youtube.com/playlist?list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX>)

In this reference we refer to episodes that require some text editing or commands in terminal.


*Technical info:
In this tutorial we have used a Ubuntu1404LTS VM, using 1 core, 1024MB Ram with a ESXi 6 hypervisor installed on a MacMini 6,2*


---
# Docker commands

### Basic commands for docker-compose
Here is a short overview of basic commands to use docker-compose.

#### Start Zentral with docker compose
```
docker-compose up -d
```

#### For a complete restart of Zentral
```
docker-compose stop
docker-compose up -d
```

#### Verify containers running docker-compose
```
docker-compose ps
```

#### Stop & remove all containers (you will loose previous data !!!)
*Attention this command will destroy data*

```
docker-compose stop && docker-compose rm -f
```

---
## Tutorial - Episode 01 - Setup/Docker: Start Zentral with docker-compose

[Tutorial Video](<https://www.youtube.com/watch?v=gmF0jaDbZ7o&index=3&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX>)

Here are the essential commands to start Zentral with docker-compose (see Docs <https://docs.docker.com/compose/>)
Please choose your docker host as discussed over at <https://docs.docker.com/>

### Install latest docker
```
curl -sSL https://get.docker.com | sh
```

### Install docker-compose binary
```
curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```
```
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

### Clone zentralsource
```
git clone https://github.com/zentralopensource/zentral.git
```

### Start zentral (everything) with docker compose
```
docker-compose up -d
```

---
## Tutorial - Episode 05 - Setup/Actions: enable Zentral notifications to Slack, use Slack WebHooks

[Tutorial Video](https://www.youtube.com/watch?v=YSjdgO7p2mU&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=7)

An example action for a Slack WebHook, insert your edited version to `base.json`

*Tip: make sure your webhook URL looks slightly different to the one in our example below!*

### Enable slack notification action
```
"slack_notify": {
  "backend": "zentral.core.actions.backends.slack",
  "webhook": "https://hooks.slack.com/services/UAQK394HL/EkB3UC9X/STRsga981jszEbcKxFxAnQGNIf"
},
```

---
## Tutorial - Episode 06 Setup/Santa: Create your first probe, use santactl to checksum binaries

[Tutorial Video](https://www.youtube.com/watch?v=2YEdNs6VwTs&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=8)

Read the fileinfo for a App with santactl binary:  `santactl info /Applications/Firefox.app`

### Example santa blacklist probe (JSON format)

Probe for a Santa blacklist of Firefox 46.0.1 on OS X. Always save a json probe with a `.json` file extension.

```
{"name":"Santa Blacklist Firefox 46.0.1 - sha256 based",
 "santa": [
	 {"sha256": "b106238716b124b107a761f3adceed90af5d53b738948f400545dcc00232f90a",
		 "policy": "BLACKLIST",
		 "rule_type": "BINARY",
		 "custom_msg": "No more Firefox 46.0.1 today - Santa blacklist"
	 }
 ],
 "actions":{
	 "slack_notify": {}
 }
}
```

To reverse a blacklisted application with Santa you **must explicitly whitelist it** again.
Below example to whitelist Firefox 46.0.1 on OS X again.

```
{"name":"WHITELIST Firefox 46.0.1 - sha256 based",
 "santa": [
   {"sha256": "b106238716b124b107a761f3adceed90af5d53b738948f400545dcc00232f90a",
    "policy": "WHITELIST",
    "rule_type": "BINARY",
    "custom_msg": "This should not be blocked"
    }
 ],
 "actions":{}
}
```
---
## Tutorial - Episode 07 - Probe/Santa: use Santa blacklisting, test a Probe and notify to Slack

[Tutorial Video](https://www.youtube.com/watch?v=8o8EwaaHOrU&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=9)

Enforce a santa sync with Zentral from terminal:  
```
santactl sync
```

---
## Tutorial - Episode 08 - Setup/External: add extra links to Zentral webinterface

[Tutorial Video](https://www.youtube.com/watch?v=aVBGjtuy6EU&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=10)


---
## Tutorial - Episode 08 - Setup/External: add extra links to Zentral webinterface
[Tutorial Video](https://www.youtube.com/watch?v=C7W0v94Pv74&index=12&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX)



## Tutorial - Episode 10 - Inventory/Sal: setup Sal-Scripts on client, merge Business Unites in Zentral

[Tutorial Video](https://www.youtube.com/watch?v=C7W0v94Pv74&index=12&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX)

#### Connect with inventory

Sal integration a multi-tenanted reporting dashboard for Munki - <https://github.com/salopensource/sal>

```
{
  "private_key": "whefhuef5263rqa0bb7f94grrf68u1m231927smcaefurgoup2yfziqw5vfajfcsmpcq",
  "public_key": "wqet9xqwewqe06nbww6rzs40fo",
  "secure": false,
  "host": "sal.example.com",
  "backend": "zentral.contrib.inventory.clients.sal"
}
```

---
## Tutorial - Episode 11 - Inventory/Watchman: setup API integration with Watchman, install Watchman on client
[Tutorial Video](https://www.youtube.com/watch?v=IPL03ebYcd4&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=13)

#### Connect with inventory
Watchman integration - <https://www.watchmanmonitoring.com/>

```
{
  "api_key": "37423xngfsvgsbqE4Gql5nIgsgsEdxrO_A",
  "account": "your_slack",
  "backend": "zentral.contrib.inventory.clients.watchman"
}
```
---

## Tutorial - Episode 12 - Inventory/JAMF: setup API integration with JAMF CasperSuite, link to Groups and Management
[Tutorial Video](https://www.youtube.com/watch?v=C7W0v94Pv74&index=12&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX)

#### Connect with inventory
Casper Suite integration - <http://www.jamfsoftware.com/products/casper-suite/>

#### JAMF inventory via API
```
{
  "password": "api_password",
  "user": "api_user",
  "path": "/JSSResource",
  "port": "8443",
  "host": "jss.example.com",
  "backend": "zentral.contrib.inventory.clients.jss"
}
```

---
## Tutorial - Episode 13 - OSQuery/Santa: detect KeRanger Ransomware, block binary notify with Zentral
[Tutorial Video](https://www.youtube.com/watch?v=GzZewrXbO-s&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=15)

A probe for detecting KeRanger with osquery. Actions with slack and change of a JSS group

```
{
  "name": "KeRanger_osquery_alert",
  "actions": {
    "slack_notify": {},
    "inventory_group": {"group_name":"quarantine_group"}
  },
  "osquery": {
    "schedule": [
      {
        "query": "select * from processes where name = 'kernel_service';",
        "interval": "30",
        "description": "http://researchcenter.paloaltonetworks.com/2016/03/new-os-x-ransomware-keranger-infected-transmission-bittorrent-client-installer/",
        "value": "Artifact used by this malware"
      },
      {
        "query": "select * from file where path like '/Users/%/Library/.kernel_%' union select * from file where path like '/Users/%/Library/kernel_service';",
        "interval": "30",
        "description": "http://researchcenter.paloaltonetworks.com/2016/03/new-os-x-ransomware-keranger-infected-transmission-bittorrent-client-installer/",
        "value": "Artifact used by this malware"
      }
    ]
  }
}
```

---

## Tutorial - Episode 14 - Setup/Notify: use message overwrites, link to MunkireportPHP and MonkeyBox

[Tutorial Video](https://www.youtube.com/watch?v=BfTteF2Y62A&index=16&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX)

#### Zentral template overights
Example for MunkireportPHP - a reporting tool for munki  <https://github.com/munkireport/munkireport-php>
```
[munkireport](http://munkireport.apfelwerk.de/report/index.php?/clients/detail/{{ machine_serial_number }})
```

Example for MonkeyBox - IT Asset Management and Password Sharing Made Easy! <https://monkeybox.com/>
```
[MonkeyBox]( https://app.monkeybox.com/accounts/174/search?utf8=%E2%9C%93&q={{ machine_serial_number }} )
```

#### body example ( Django template / Jinja2 )
```
Serial number: {{ machine_serial_number }}
{% if machine_url %}Zentral URL: {{ machine_url }}{% endif %}

{% for name, source_names in machine_names.items() %}
Name: {{ name }}, ({{ source_names|sort|join(', ') }})
{% endfor %}

{% if event_id %}
Zentral event id: {{ event_id }}
Event created at: {{ created_at }}
{% endif %}

{% block extra %}
{% endblock %}
```

---

## Tutorial - Episode 15 - Setup/Docker: update Zentral with docker-compose, use GitHub for update infos

[Tutorial Video](https://www.youtube.com/watch?v=_G2LH00sJFs&index=17&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX)


#### Git pull latest source

Download latest Zentral code update from GitHub <https://github.com/zentralopensource/zentral>
```
git pull
```

Restart Zentral - rebuild after code update

```
docker-compose stop
docker-compose up -d
```

---

## Tutorial - Episode 16 - Setup/OSQuery: use osquery TLS feature only, create a minimalistic Zentral

[Tutorial Video](https://www.youtube.com/watch?v=_G2LH00sJFs&index=17&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX)


### Modular Zentral - run a minimalistic osquery configuration

#### Stop & remove all containers (be aware you will loose previous data !)
```
docker-compose stop && docker-compose rm -f
```
#### Start all over in a minimal config
```
docker-compose up -d
```

#### Minimal apps in base.json
```
"apps": {
  "zentral.contrib.inventory": {
    "clients": [
    ],
    "prometheus_push_gateway": "prompg:9091"
  },
  "zentral.contrib.osquery": {}
}
```


#### A test query
```
SELECT * FROM osquery_info;
```

---
