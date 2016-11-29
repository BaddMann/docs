![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)

## Zentral - a short summary

[Zentral](https://github.com/zentralopensource/zentral) is a open source Framework and server solution to manage configurations for osquery and santa - a build in event tracking, notification and time series event processing will complement these open source technologies.

It will enable you to gather specific information, filter events, trigger notification, into compelling event based automations and workflows.
With Zentral's orchestration of osquery and Santa you'll be empowered by having a broader source of information and knowledge about your IT infrastructure, identify and react to changes on OS X and Linux clients.

It is an open source tool that can enhance stability and security thanks to its built-in integration of existing inventory solutions that are already on the market for the Mac platform.  

Zentral will fit many kinds of public and private organizations which are either already utilizing  or allow those that are wanting to utilize the Mac platform. It is ready for use in enterprise environments and is tailored towards #macadmins that want to enhance the tools, they already know, with a dedicated open source solution for better incident and event-based management.


*Zentral is a great add on to strengthen productivity, stability, and security for the Mac platform - the full framework of Zentral will enable organizations in need, to establish a transparent, and open source Security Incident and Event Management Solution (SIEM), with outstanding OS X support.*

## Zentral Deployment
*latest update: November 2016*

For Deployment look into this guide: <https://github.com/zentralopensource/docs/blob/master/zentral-deployment.md>

For a quick AWS based setup: <https://github.com/zentralopensource/docs/blob/master/zentral-aws-setup.md>

For a quick Google Cloud based setup: <https://github.com/zentralopensource/docs/blob/master/zentral-gcloud-setup.md>

---

## Zentral Video Tutorial
*latest update: May 2016*

Please look into our Tutorial here: <https://github.com/zentralopensource/docs/blob/master/zentral-tutorial.md>

---

## FAQ
*latest update: June 2016*

You can find a FAQ section here: <https://github.com/zentralopensource/docs/blob/master/zentral-faq.md>

---

## Zentral web interface details
*latest update: April 2016*

Zentral web interface details:

- Django 1.9 Web Framework
- Bootstrap 3
- Nginx

This section is a short example overview from various areas in Zentral web interface as well as [Kibana4](<https://www.elastic.co/products/kibana>) and [Prometheus](<http://prometheus.io>)

## Zentral Inventory

#### Inventory overview

List of devices in Inventory

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_inventory_overview.png)


#### Inventory detail view

Device detail view with basic facts and apps (synced from external inventory like Sal or JAMF Software Server)

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_inventory_detail.png)

#### Inventory events

Event details on sync with external Inventory via API connection (Sal or JAMF Software Server)

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_inventory_events.png)


### Inventory Events > osquery

Details on osquery features and functionality: [osquery](<https://osquery.io>)


#### osquery results

Results from osquery **Probes** returned to Zentral.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_osquery_results.png)

#### osquery status

Status events returned from osquery as scheduled on client devices.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_osquery_status.png)

### Inventory Events > Santa

Details on Google Santa features and functionality: [Google Santa](<https://github.com/google/santa>)

#### santa events

Events returned from Google Santa binary on client device.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_santa_events.png)

#### santa sync

Sync details returned from Google Santa binary on client device.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_santa_sync.png)


### Zentral Probes

#### Probes view

##### osquery probes
Zentral Probe detail view (no editing in Web UI currently) with direct link to Kibana4 UI [Elasticsearch button].

Notification actions for this osquery probe will send notification to:

- Slack notification
- Zendesk ticket creation
- JSS API - Group membership in JSS will changed as result (Demo video: [Enforce JAMF MDM trigger with Zentral and osquery](<https://youtu.be/hdDoWK0A9TQ>) )
- SMS sended via Twilio gateway


![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_probe_usb_zentral.png)

Zentral Probe for community pack with direct link to Kibana4 UI [Elasticsearch button].

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_osquery_packs.png)

Zentral Probe for FIM with file paths to monitor.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_osquery_fim.png)

##### Santa probes
- Slack notification
- Push notification to iOS via [Pushover]

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_probe_santa_zentral.png)

### Zentral distributed query

Results for a osquery distributed query (details: [osquery distributed](<https://osquery.readthedocs.org/en/latest/installation/cli-flags/#distributed-flags>) )

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_distributed_query_results.png)

Create a new distributed query

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_distributed_query_create.png)

#### Kibana / Elasticsearch

Probe results displayed in Kibana4 UI with details from the Elasticsearch database.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_probe_usb_kibana.png)



## Prometheus

#### Application time series view

Time series overview of Application version changes (Firefox as example) sourced from all client devices.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_prometheus_apps.png)

#### OS X versions time series view

Time series overview of OS X version updated sourced from all client devices.

![](https://github.com/zentralopensource/docs/blob/master/images/zentral_ui_prometheus_osx_vers.png)

---
