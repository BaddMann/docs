![](https://github.com/zentralopensource/zentral/wiki/images/Zentral_base_RGB.png)

# Zentral

## What is Zentral?

[Zentral](https://github.com/zentralopensource/zentral) is a new open-source project initiated by Apfelwerk in Summer/Fall 2015. [Zentral](https://github.com/zentralopensource/zentral) combines [osquery](https://osquery.io/)'s powerful endpoint inventory features with a flexible notification and action framework. This enables one to identify and react to changes on OS X and Linux clients. Zentral consolidates the osquery information with inventory data from client management suites, e.g. JAMF Casper Suite. All details and events stored in a full text search engine.

## Features
Zentral's main features are:

- Gateway to connect a full stack of open source software
- Provide multiple configurations for [osquery](https://osquery.io/) via a pull model over HTTPS
- Combine [osquery](https://osquery.io/) results with flexible notifications and configurable actions
- Metrics stored into in a full text search engine for time based processing
- Connects with existent inventory, for OS X clients managed with Munki (SAL) or JAMF CasperSuite 

## Components
Zentral follows a modular approach and consists of multiple components. At it's core it is the intelligence to provide filtering and is a processing framework to interface with other tools. A best breed of open source tools deployed with Zentral Core to a operational state (provided as Docker container and deployable via Ansible) when you start with Zentral.

- A Core Framework to connect with other resources 
- [Redis](<http://redis.io>) in-memory cache, used for event queues.
- [PostgreSQL](<http://www.postgresql.org/docs/>) persistent database for inventory and metadata.
- [ElasticSearch](<https://www.elastic.co/products/elasticsearch>), full-text search database for all event data
- [Kibana4](<https://www.elastic.co/products/kibana>), real-time analytics and visualization platform for events 
- [Prometheus](<http://prometheus.io>), monitoring system with a time series database 

## Architecture

This diagram illustrates the main architecture of Zentral and some of its ecosystem components:

![](./overview.png)

Zentral is build in a modular fashion, functional elements can expand, scale, load balanced or replaced by similar tools where needed.

## When does Zentral fit?

If you're a small to medium sized IT team, responsible for a fleet of mac clients and/or linux clients/servers. If you'd eager to combine the power of osquery, existing inventory, make use of time based event metrics over your fleet of clients into a new breed of flexible alerting and monitoring.

If you like to have a starting point, integrate osquery with your solutions already in place, extend and build up on the modular structure of Zentral. 


## When does Zentral may not fit?

If you're a large enterprise that has dedicated budget and resources for big data analytics 