# Introduction

[Zentral](https://github.com/zentralopensource/zentral) is running in a Django web framework - the **Zentral/osquery App** operates as a TLS endpoint for osquery, **Zentral/santa App** as a TLS endpoint for Google Santa, **Zentral/munki App** for Munki to connect with.

Clients devices must have installed the according technology before they can connect with Zentral.

- Clients running [osqueryd](https://osquery.io/) and enrolled with Zentral can pull osqueryd configurations and push results back to Zentral.

- Clients running [Santa](https://github.com/google/santa) and enrolled with Zentral can pull Santa policies and push results back to Zentral.

- Clients running [Munki](https://www.munki.org/munki/) and enrolled with Zentral can push inventory data and events history to Zentral.

---

## How to enroll OS X client devices with Zentral for osquery, Santa and Munki?

Since late 2015 Zentral supports a simple way to enroll OS X clients by providing a prebuild OS X packages for each technology to download from the Zentral web interface.

##### There's four conditions you have to keep in mind:

1.) Inventory is organized in BusinessUnits, depending on your use you ma have just one or many BusinessUnits. In any case you should enroll a client to a selected BusinessUnit. Zentral will build a dedicated package per technology, when downloading just select the BusinessUnit accordingly from the menue.


2.) For osquery, Google Santa to be able to pull configurations and push results back to Zentral they need to be enrolled separately. Same will apply for Munki which pushes local inventory and events history to Zentral. The packages to enroll clients are downloaded in the Zentral web interface from the **Enroll** menue of the selected technology (osquery, Santa, Munki).

3.) Clients devices must have installed the according technology before they can connect with Zentral. Please install latest version of osquery, Santa or Munki first befor you'll try to enroll a client with Zentral.

4.) The packages downloaded to enroll with Zentral are normal *unsigned* flat-packages for OS X. It's up to you how to deploy these packages to your clients.
