# Santa

Santa is a binary whitelisting/blacklisting system for Mac OS X. 

> It consists of a kernel extension that monitors for executions, a userland daemon that makes execution decisions based on the contents of a SQLite database, a GUI agent that notifies the user in case of a block decision and a command-line utility for managing the system and synchronizing the database with a server.


Santa is a security software from Google available as open source, read more details [here](https://github.com/google/santa/blob/master/README.md)
[Zentral](https://github.com/zentralopensource/zentral) priovides alpha support for [Santa](https://github.com/google/santa) as a central management server, clients can synchronize via TLS with a Zentral. 



## Brief Overview 

In Zentral - a Probe will be used set **rule** for `santa` along with dedicated notification settings for information santa returns from the client. The client will connect to zentral via santa conf and frequently updates (sync) the santa status via TLS connection.


*Note:*
*This is only a brief tutorial on **Google Santa** -  santa is currently v.0.9.6 and it's support in Zentral is considered early tech preview in 'alpha status'. We will update the content for changes in santa binary whenever possible.*

### Santa Prepare

Binaries can be whitelisted/blacklisted by their signing certificate, required is to provide a SHA-256 unique hash either from the Code-signed signing chain or the SHA-265 calculated for the App bundle. Each version of an app will have different SHA-256, i.e. Firefox 42.0 will have a different one to Firefox 42.0.1 - obviously using the SHA-256 for Code-signed certs for Firefox has some benefit here.

You have these options:

- SHA-256 from Application path 
- SHA-256 from Code-signed certs


**Caution:**  
*Please do not blacklist the Apple Root cert with santa - this could block important OS X system components.*


#### Evaluate the SHA-256 hash

Practically to check the SHA-265 for Firefox.app us the `santactl` binary in Terminal:

> `santactl binaryinfo /Applications/Firefox.app`


The results for Firefox 42.0 should look like this:

```
santactl binaryinfo /Applications/Firefox.app
Path                 : /Applications/Firefox.app/Contents/MacOS/firefox
SHA-256              : de5d5662c8d8390c82ebe1235e700babccecd1d998f79ab9cc4e0a04d0c5ea5c
SHA-1                : 179e35e247a0eaf772669127066107fea55cdd7c
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

     2. SHA-256             : 7afc9d01a62f03a2de9637936d4afe68090d2de18d03f29c88cfb0b1ba63587f
        SHA-1               : 3b166c3b7dc4b751c9fe2afab9135641e388e186
        Common Name         : Developer ID Certification Authority
        Organization        : Apple Inc.
        Organizational Unit : Apple Certification Authority
        Valid From          : 2012/02/01 23:12:15 +0100
        Valid Until         : 2027/02/01 23:12:15 +0100

     3. SHA-256             : b0b1730ecbc7ff4505142c49f1295e6eda6bcaed7e2c68c5be91b5a11001f024
        SHA-1               : 611e5b662c593a08ff58d14ae22452d198df6c60
        Common Name         : Apple Root CA
        Organization        : Apple Inc.
        Organizational Unit : Apple Certification Authority
        Valid From          : 2006/04/25 23:40:36 +0200
        Valid Until         : 2035/02/09 22:40:36 +0100
```



## Create a Probe to Blacklist Firefox

The probe has to be loaded with the zentral-conf, read more [here](<https://github.com/zentralopensource/docs/blob/master/zentral-create-probes.md>)

```
{"name":"testsanta",
 "santa": [
   {"sha256": "de5d5662c8d8390c82ebe1235e700babccecd1d998f79ab9cc4e0a04d0c5ea5c",
    "policy": "BLACKLIST",
    "rule_type": "BINARY",
    "custom_msg": "Zentral - Firefox binary blacklist test"
    }
 ],
 "actions":{
   "slack_macops": {},
   "pushover": {"groups": ["admin"]}
 }
}
```

### Santa parameters

* sha256
    * from Bundle
    * from Signing chain

* policy
    * BLACKLIST
    *

* rule_type
    * BINARY 
    * CERTIFICATE


* custom_msg
   * A custom message

---
