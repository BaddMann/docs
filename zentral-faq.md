![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)
# Zentral FAQ

Frequently asked questions about Zentral are updated here.

## What is Zentral ?

Zentral is a open source Framework and server solution to manage configurations for osquery and santa - a build in event tracking, notification and time series event processing will complement these open source technologies into a powerful event monitoring and infrastructure analyzation Framework tailored for organizations with Mac clients and Linux infrastructure.
It is a dedicated solution for an re-active incident and event management in common IT infrastructure.

## What can it do for my organization ?

Zentral was started as a concrete answer how to run a TLS server for osquery configurations - osquery can easily ask questions about your Linux and OSX infrastructure. Next to osquery Zentral supports another security centric tool Santa build by the macops team at Google. To bring both powerful tools into a productive workflow we build up a modular Event-Filter-Action Framework into Zentral, and integrate with existing inventory and software management solutions. Actions can trigger notifications for filtered events, post incidents to popular ticket systems, email, SMS, push notification, Kanban boards - the notification actions supports webhooks. Thanks to the Python language Zentral is easily extendable to integrate with enterprise systems, or systems for operations, system or service management already in place.

## How can I get started ?
We provide a multi docker container setup - so you will be up in few minutes with a docker-compose file. This setup for testing, development and production use - we currently run a production Zentral in a docker-compose setup along with the more resource hungry Kibana4 on a 4CPU, 8 GB Ram SSD cloud server.

Simply get started with Zentral check out the video tutorials here: <URL>

## how does it work ?
Zentral is written in Python3 and runs in a Django 1.9 Web-Framework as TLS endpoint for clients devices to connect and report to. The technology stack on the client side includes osquery, Google Santa, and munki. On server side a Nginx Webserver, a queue system, a elastic full text searchable database is used to store events. Zentral connects to existing inventory (JAMF JSS, Watchman) The Event-Filter-Action Framework will enable you to filter events from multiple sources, tags help to set scope of probes - a combination of filters, queries, policies and actions.

## It sounds allover complex - can I just have one feature of it ?
Yes one’s use of tools is very different and thus Zentral is build in a highly modular fashion. If you want to run a minimalistic setup it’s a simple change in the `base.json` config to minified Zentral to a OSQuery or just a Santa TLS server. Adding integrations or re-enabling features is as simple as going minimalistic - just edit the `base.json` file and restart Zentral services.


## Zentral is running over here, but why is there no Kibana4 included ?
Glad you asked - yes the docker-compose setup for Zentral is up and running in minutes,  and includes the ElasticSearch database so of course Kibana4 is a natural fit to the technology stack of Zentral. Zentral can easily connect to Kibana4, which is pretty resource hungry. so you may want to check your resources on server / docker host. Once you’ve ready, and installed Kibana (most will run Kibana in a docker container here too) you can connect with Zentral via this docker command:

```shell
sudo docker run --link zentral_elastic_1:elasticsearch -p 5601:5601 --rm kibana
-> http://localhost:5601
```

## How can I get a full overview and start to learn about my IT environment with Zentral ?

1) Look to a 10 min walk thru screencast from the Mac Brains - [Hack The Mac contest] (<https://www.youtube.com/watch?v=bDWFRntc2eQ&list=PLv__E26s_yEy5TwVRwAb-99_MA20BGhgX&index=1) in November 2015. In case you want to catch up with basic facts about *osquery, Santa, Zentral*

2) Dedicate 1h of your time, start the Zentral docker-compose tutorial with a 17 track video tutorial here:  <https://github.com/zentralopensource/docs/blob/master/zentral-tutorial.md>

## We have a commercial SIEM tool but are unsatisfied how to integrate Macs.

Zentral is a highly modular framework and can serve multiple purpose - gather specific information, filter events, trigger notification, gain broader source of information and knowledge of what's going on in your Mac IT environment over time - depending on your specific needs you can quickly build a simple to use but tailor made Security Incident and Event Management (SIEM) solution that targets to the Mac platform.
A common example is to manage and add osquery and santa security technologies with Zentral and the JAMF Casper Suite -
new options for event based automation, monitoring capabilities, granular notifications on events will benefit from a tight integration with the JAMF Software Server (JSS) and connect MDM capabilities with the Zentral framework.

## Can we to use our existing MDM solution with Zentral ?

Yes, you can use your existing MDM in parallel (AirWatch, Afaria, MobileIron, InTune, etc.).

We use the JAMF Casper Suite as our MDM with compelling event automations, osquery can manage groups in the JSS, we manage black/whitelisting with santa, query specific information, identify and react to changes, filter on events and notify the JSS API.

Zentral can coexist with any MDM or integrate with specific MDM by building a `contrib app` - special code written in python - like we already show with the JAMF JSS. To build a similar integration for your MDM of choice it must have a documented Application Programming Interface (API) - contact us for consulting and services to help you here.

As we very much love open source software we're extremly happy to see a new open source MDM <https://micromdm.io/> become ready and will look into options to integrate with for the future.
