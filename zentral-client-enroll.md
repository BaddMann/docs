# Introduction

[Zentral](https://github.com/zentralopensource/zentral) is a web server based on Django. Clients devices must be enrolled to Zentral to manage configurations and receive events from OSQuery, Santa, and receive inventory from Munki managed clients. Next to the client based technologies Zentral can connect also to other inventory systems like JAMF CasperSuite, Watchman Monitoring, or Sal ,and sync inventory data for clients via API.
Events received from each inventory and clients will be indexed, processed and stored with additional metadata in a fully searchable database. 

Your macOS client devices need to have the supported technologies [OSQuery](https://osquery.io/), [Google Santa](https://github.com/google/santa), and [Munki](https://www.munki.org/munki/) agents installed first, these will connect to Zentral by installing a dedicated Zentral enrollment package- Zentral itself is "agentless", it acts as a centralized TLS server to provide API endpoints for supported technologies.


**Zentral - TLS endpoint for [osquery](https://osquery.io/)**  

- Clients running [osqueryd](https://osquery.io/), enrolled to Zentral will pull osqueryd configurations and push back results to Zentral
- The base config for `osqueryd` on macOS / OS X based clients is stored in `/Library/LaunchDaemons/com.facebook.osqueryd.plist`
- The required certificate used by osquery is stored at `/usr/local/zentral/tls_server_certs.crt`


**Zentral - TLS endpoint for [Google Santa](https://github.com/google/santa)** is a TLS endpoint for Google Santa

- Clients running [Santa](https://github.com/google/santa), enrolled with Zentral can pull Santa policies and push results back to Zentral.
- Path for osqeryd config on macOS clients is `/var/db/santa/config.plist`


**Zentral - endpoint for [Munki](https://www.munki.org/munki/) local inventory** 


- Clients running [Munki](https://www.munki.org/munki/), enrolled with Zentral can pull Santa policies and push results back to Zentral.
- Path for munki postflight script on macOS clients is `/usr/local/munki/postflight.d/zentral` symlinked to /usr/local/zentral/munki/zentral_postflight`


---

## How to enroll OS X client devices with Zentral for osquery, Santa and Munki?

Since late 2015 Zentral supports a simple way to enroll OS X clients by providing a prebuild OS X packages for each technology to download from the Zentral web interface.

##### There's four conditions you have to keep in mind:

1.) Inventory is organized in BusinessUnits, depending on your use you ma have just one or many BusinessUnits. In any case you should enroll a client to a selected BusinessUnit. Zentral will build a dedicated package per technology, when downloading just select the BusinessUnit accordingly from the menue.


2.) For osquery, Google Santa to be able to pull configurations and push results back to Zentral they need to be enrolled separately. Same will apply for Munki which pushes local inventory and events history to Zentral. The packages to enroll clients are downloaded in the Zentral web interface from the **Enroll** menue of the selected technology (osquery, Santa, Munki).

3.) Clients devices must have installed the according technology before they can connect with Zentral. Please install latest version of osquery, Santa or Munki first befor you'll try to enroll a client with Zentral.

4.) The packages downloaded to enroll with Zentral are normal *unsigned* flat-packages for OS X. It's up to you how to deploy these packages to your clients.
