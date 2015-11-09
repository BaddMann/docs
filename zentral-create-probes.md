# Zentral Probes

## Overview
Zentral introduces the concept of probes, which combine osquery and santa results with configurable notifications and actions.
Each Probe will provide a configuration the clients will sync with.

* osquery or santa rule to distribute 
    * osquery - single query 
    * osquery - pack (see community packs: <https://osquery.io/docs/packs/>) 
    * osquery - fim file integrity monitoring
    * santa - rule for white/blacklisting

* notifications / actions
    * notify a backend:
        * Slack
        * email
        * SMS
        * iOS push notification
        * create file in filesystem (as referenced in zentral-conf: base.json)

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_probe_usb_zentral.png)    
    
    
## Probe Details

Probes are simple text files in the `Probes` directory of your `zentral-conf` on the Zentral server. The files must be written in JSON or YAML format. Currently there is no autoupdate of new/edited probes files - any changes have to be reloaded in Zentral with Supervisor or a Docker restart (both options are very fast).

Important: If the JSON or YAML formatting has any errors Zentral will fail to load, so please double check any probe with a text linter to verify your JSON or YAML files are correctly formatted.

*Tip:*
Debugging errors here could be frustrating as it's often just a space, to many or missing characters like: `,` or `}` as source of errors. We recommend to use free Atom Text Editor from GitHub <https://atom.io> and install these Packages for checking your JSON and YAML integrity:

- linter-js-yaml <https://atom.io/packages/linter-js-yaml>
- linter-jsonlint <https://atom.io/packages/linter-jsonlint>



## Some examples for probes

Here is a overview with few example probes in YAML and JSON format.


---

### Kext Probe
This Probe is written in YAML format.

##### Description:
Will use `osquery` SQL syntax : 

- The probe will monitor all file changes in `/Users/Shared/` directory including subdirectories.


##### Schedule:

- osquery: `SELECT * FROM kernel_extensions WHERE name LIKE '%LittleSnitch%';`
- interval:  every hour (equals to 3600 sec)

##### Actions are:

- notification to slack 
  (referenced as `slack_chn1` in in `base.json` conf file)

- file created in `little_snitch` sub directory in `"local_dir": "/tmp/zentral_notifications/"` 
  (referenced in `base.json` conf file)



```
name: little_snitch-kext_alert
osquery:
  schedule:
    - query: SELECT * FROM kernel_extensions WHERE name LIKE '%LittleSnitch%';
      interval: 3600
actions:
  debug:
    sub_dir: little_snitch
  slack_chn1: null
```

---

### FIM Probe
This Probe is written in YAML format.

##### Description:
Will use `osquery` file integrity monitoring: 

- The probe will monitor all file changes in `/Users/Shared/` directory including subdirectories.


##### Schedule:

- osquery: `select * from file_events where category='users_shared';`
- interval: interval of 30 sec
- osquery file_paths: key is: `users_shared:` and value is: `/Users/Shared/%%` 
(the `'%%'` is a wildcard in osquery FIM)

##### Actions are:

- notification to slack 
  (referenced as `slack_chn1` in in `base.json` conf file)
  
- file created in `fim` sub directory in `"local_dir": "/tmp/zentral_notifications/"` 
  (referenced in `base.json` conf file)


```
name: fim
osquery:
  schedule:
    - query: select * from file_events where category='users_shared';
      removed: false
      interval: 30
  file_paths:
    users_shared:
      - /Users/Shared/%%
actions:
  test:
    sub_dir: fim
  slack_chn1: null
```

---

### Google santa Probe
This Probe is written in JSON format.

##### Description:
Will use `santa` to blacklist execution for a certificate.
Send the users launching Mozilla signed software a custom message in the Santa GUI (on the client).
Notify users have launched Mozilla signed software with details to Slack and iOS push notification to all admins.

- Every software on the client will be blocked in execution


```text
Bundle Name          : Firefox
Bundle Version       : 4215.10.29
Bundle Version Str   : 42.0
Type                 : Fat Binary (x86-64, i386)
Code-signed          : Yes
Signing chain:
     1. SHA-256             : b106238716b124b107a761f3adceed90af5d53b738948f400545dcc00232f90a
        SHA-1               : f24365e6d31cbe67d4d5b5179c0ce092c1acd27d
        Common Name         : Developer ID Application: Mozilla Corporation
        Organization        : Mozilla Corporation
        Organizational Unit : 43AQ936H96
        Valid From          : 2012/05/22 19:42:20 +0200
        Valid Until         : 2017/05/23 19:42:20 +0200

```


##### Schedule:

- SHA-256: used from Signing chain with Common Name "Developer ID Application: Mozilla Corporation"
- policy: "BLACKLIST",
- rule_type: "CERTIFICATE",
- custom_msg: "Zentral - Mozilla signing cert blacklist test"

##### Actions are:

- notification to slack 
  (referenced as `slack_chn2` in in `base.json` conf file)

- iOS push notification via Pushover send to all contacts in group `admin`  
  (referenced in `base.json` and `contacts.yml` conf file)

```
{"name":"testsanta_certbased",
 "santa": [
   {"sha256": "b106238716b124b107a761f3adceed90af5d53b738948f400545dcc00232f90a",
    "policy": "BLACKLIST",
    "rule_type": "CERTIFICATE",
    "custom_msg": "Zentral - Mozilla signing cert blacklist test"
    }
 ],
 "actions":{
   "slack_chn2": {},
   "pushover": {"groups": ["admin"]}
 }
}

```

---


