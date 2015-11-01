![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)

# Zentral Docker Tutorial

## Overview
[Zentral](https://github.com/zentralopensource/zentral) is a new tool in the *#macadmins* ecosystem it is in heavy development so expect changes and additions in the future.
To evaluate, learn and quickly deploy Zentral you should run [Docker](<https://docs.docker.com>) and start with the `Zentral Docker image` posted on Docker Hub: <https://hub.docker.com/r/zentral/zentral/>

This Tutorial will guide you to:

- Pull the `zentral/zentral` image from Docker Hub.
- Create a custom `my-zentral-conf` Zentral configuration.
- Run Zentral in Docker, check your `my-zentral-conf`
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
Start the Tutorial:

- [Pull the Docker image from Docker Hub](<https://github.com/zentralopensource/docs/blob/master/zentral-docker-tutorial_1.md>).
- [Create a custom Zentral configuration](<https://github.com/zentralopensource/docs/blob/master/zentral-docker-tutorial_2.md>).
- [Run Zentral in Docker with custom config](<https://github.com/zentralopensource/docs/blob/master/zentral-docker-tutorial_3.md>)
- [Run Zentral in Docker with Debugging enabled](<https://github.com/zentralopensource/docs/blob/master/zentral-docker-tutorial_4.md>).
- [Enroll a OS X client from Terminal](<https://github.com/zentralopensource/docs/blob/master/zentral-docker-tutorial_5.md>)
- [Login to Docker host to debug Actions](https://github.com/zentralopensource/docs/blob/master/zentral-docker-tutorial_6.md)

--- 

## Wrap Up


This is a first tutorial on Docker and Zentral. 
Thanks you taking time to try out Zentral in Docker.
We hope it's now even more fun to run osquery on and you will soon come up with cool ideas using Zentral and osquery in your environment.
Of course we can not cover all details in a single Tutorial, there's much more options running Zentral in Docker, we just did a short cut to get you started, we've embedded al the tools into one Docker image - to scale up that could be a bottle neck depending on your Docker host. 

We would love to see your contribution, submit errata, comments for improvements, etc.

Some areas ask for specific contributions:

- Instructions or a Tutorial to run `zentral/zentral` docker image behind a Docker reverse proxy for TLS termination.
(Yes we know about this project <https://github.com/jwilder/nginx-proxy> but did not have enough time to play with)

- Extended Tutorial for Kibana4 data visualization. 

- Create Probes with useful `osquery` definitions and share with the community. 
(For the future of Zentral we hope to learn more how Greg Neagle and Tim Sutton handle the community submissions for AutoPKG)

- Code contribution - start to look into the [Zentral source code](https://github.com/zentralopensource/zentral). 
Create and contribute additional Actions (Hint: Slack is cool, but what about HipChat?).


## Thank You

A special thanks goes to: 
Allister Banks, Marko Jung, Tim Sutton, and Arek Dryer for kicking the wheel during MacSysAdmin 2015. 
To Graham Gilbert for help with changes in Sal API.
Teddy Reed and Mike Arpaia from Facebook/osquery for their amazing work. 
Last but not least to *you* for starting with Zentral.

---
