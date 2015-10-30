![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)

# Zentral

**NOTE**:
This file is a few weeks old and will not be part of the active maintained documentation.
As infos contained here are not outdated we keep it for reference for a while.


## What is Zentral?

Zentral combines [osquery](https://osquery.io/)'s powerful endpoint inventory features with a flexible notification and action framework. This enables one to identify and react to changes on OS X and Linux clients. Zentral consolidates the osquery information with inventory data from client management suites, e.g. JAMF Casper Suite. All details and events are stored in a full text search engine.


## What does it do?

Zentral introduces the concept of probes, which combine osquery results (??) with configurable notifications and actions. The installation of osquery and enrollment in Zentral can be automated using all major OS X management solutions. The individual tests are distributed by the Zentral server. Every client regularly executes the tests according to a configurable schedule. Any state change is reported back to Zentral as an event. Inventory updates are treated as events as well. All events are logged, filtered, and stored in a database.

Zentral is able to inform a broad set of stakeholders within an organisation using text messages (SMS), emails, push notifications, or Slack messages.

Zentral can trigger automated actions like the creation of an incident ticket or infrastructure event in an incident management system, or interactions with third party APIs. For any given probe there are no restrictions on the amount of notifications and interactions. This allows one to inform a computer emergency response team (CERT) whilst initiating a fully automated response to the same incident.

The modular design of Zentral allows the easy addition of actions (e.g. notifications, automated responses), filters, and storage backends. [Elasticsearch](https://www.elastic.co/products/elasticsearch) as the default backend provides a powerful foundation to store and process all inventory data and osquery events. Zentral currently provides preconfigured hooks into the [Kibana](https://www.elastic.co/products/kibana) and [Prometheus](http://prometheus.io/) visualization tools.


## How is this useful?

Zentral can be seen as a reliable tool to integrate the fast growing but changing osquery tool into an existing client management environment.

Zentral complements existing client management solutions by introducing the unique ability to query client state and inventory information independently to provide near real-time actions to react on events. The notifications and actions per probe are highly configurable with the aim to implement any kind response to state changes. 


## Why is Zentral built this way?

Our goal is to enable everyone to leverage osquery regardless of the existing client management environment. We do not want to reinvent the wheel – Zentral aims to interface with existing tools instead of building another large monolithic tool. 

Zentral facilitates the best of breed open source tools for its query-filter-act workflow. The system architecture is very modular to enhance, add, and replace individual components like using a simple Unix pipe.

Zentral is open source software.


## Status

Zentral is currently under development and still far from being stable or ready to be used in production. Inventory can be taken from a JSS, using Munki as an inventory source is under development. Current notifications are Slack and JSS API calls. Visualization is Kibana and Prometheus.

We are considering many other notifications and sources for inventory. We are happy about any feedback and ideas or contributions in these areas. We would be exceptionally happy to hear from anyone into visualization.

Please tell us your ideas, how a use case for you might look like and what you would like to see as an addition.


## Example use cases:

**Example 1:** When a rogue USB device is detected, access to VPN or other corporate devices is revoked.

**Example 2:** When FileVault is disabled by the user, a specified folder containing sensitive files is deleted and the admin notified

**Example 3:** Third-party installed software that doesn't get used after 30 days can be automatically slated for removal in munki.



## Example Demo Video:

Early alpha version connecting to JAMF Casper Suite Inventory and JSS API.

Enforce JAMF MDM trigger with Zentral and osquery:

<https://youtu.be/hdDoWK0A9TQ>

