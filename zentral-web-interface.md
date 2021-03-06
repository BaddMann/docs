![](https://github.com/zentralopensource/zentral/wiki/images/Zentral_base_RGB.png)

# Zentral web interface details
Zentral web interface details:

- Django 1.9 Web Framework
- Bootstrap 4
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
