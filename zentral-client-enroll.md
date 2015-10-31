# Introduction

[Zentral](https://github.com/zentralopensource/zentral) is running in a Django web framework - the **Zentral/osquery App** operates as a TLS endpoint for your osquery clients devices to connect with. 

Clients running [osqueryd](https://osquery.io/) and enrolled with Zentral can pull osquery configurations and push results back to Zentral. 

To quick start or debugging you can run the `osqueryd` binary with required arguments in a Terminal session (see example below) - but obviously that method would not scale for a fleet of devices.

For OS X clients devices to connect to with [Zentral](https://github.com/zentralopensource/zentral) you must have [osquery](https://osquery.io/) installed (osquery v.1.5.3 or later) and create a LaunchDaemon to run the `osqueryd` binary with all required arguments, `osqueryd` will *enroll* the device and direct all traffic (TLS encrypted) to your own instance of Zentral.

---

## How to enroll OS X client devices with Zentral/osquery?
To to help you get started to create a LaunchDaemon with all settings, enroll and connect your devices with **Zentral/osquery**, we provide a build script to build a installer package as *.pkg* file. 

Once you have created the package (.pkg) you have to choose your own method to install/deploy the installer package to your devices (munki, Casper, etc.).

**Requirement:** 
To run the script provided to build your custom OS X Zentral Enrollment package, it is required to use [munkipkg](<https://github.com/munki/munki-pkg>), a simple and great tool by Greg Neagle (creator of munki) - [munkipkg](<https://github.com/munki/munki-pkg>) provides a great amount of flexibility building packages (it use Apple `pkgbuild` under the hood), simplify code signing and enables to commit your sources into a git repository (great to track changes).


### Steps to create a Zentral Enroll Package

To run the `Zentral-enroll-build.sh` script you must have [munkipkg](<https://github.com/munki/munki-pkg>) utility installed. 
Read more details here: <https://github.com/munki/munki-pkg>


1. First you need to downloaded the [munkipkg](<https://github.com/munki/munki-pkg>) utility.
1. Change `Create_enroll-pkg` parameters to match your organisation
 
2. If needed download [munkipkg](<https://github.com/munki/munki-pkg>)
3. Ensure `munkipkg` is in your path (copy to `/usr/local/bin` or reference in `Create_enroll-pkg.sh` script)
2. start the **Create_enroll-pkg** in Terminal: `./Create_enroll-pkg.sh` 


### IMPORTANT: Script Parameter to adjust:
- **ZENTRAL_TLS_HOST**
- **ENROLL_SECRET_KEY**

```
ZENTRAL_TLS_HOST="zentral.pretendco.com"
ENROLL_SECRET_KEY="FOOBARBAZ"
WORK_DIR="/usr/local/zentral" 
ENROLL_SECRET_PATH="/usr/local/zentral/enroll_secret.txt"  
LAUNCH_DAEMON_NAME="com.facebook.osqueryd"  
```
