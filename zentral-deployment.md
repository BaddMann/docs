![](https://github.com/apfelwerk/Zentral/wiki/images/Zentral_base_RGB.png)
# Deployment

We maintain different deployment methodes for Zentral.
To start with Zentral you may choose your preferred method for deployment, please see below for details.

The so far simplest solution to get started with Zentral is to run a cloud based  **"Zentral - all in one"** on Amazon AWS or Google Cloud. 
We create pre-build images and release them as public images for AWS and Google Cloud.

For a local evaluation we provide a Vagrant based **"Zentral - all in one"** based on VirtualBox. Finally we also build a OVA file that usually could be deployed to VMWare Fusion, ESXi/vSphere or other virtualization hosts.

All pre-build images are build with [Packer](https://www.packer.io/), we use [Ansible](https://www.ansible.com/) as our provisioner with Packer. 

Depending on your need to further explore, develop, and contribute back to Zentral the `docker-compose` based install method is probably the way to go.

## docker + docker-compose

This method is best suited for local evaluations and the development of Zentral. We don't recommend it for production. You will simply need a docker, docker-compose and a copy of the source code.

The docker-compose configuration includes the following containers

- Postgres 
- RabbitMQ
- ElasticSearch 5.x
- Prometheus 1.4.x
- Zentral store_worker
- Zentral inventory_worker
- Zentral processor_worker
- Zentral web 
- Nginx


*Notes: You need to make sure to have a valid TLS certificate in this setup*

## Image based

This method is best suited for production and/or tests with small fleet of devices. The pre-build "Zentral all in one" image contains all the elements of the docker-compose based deployment plus Kibana.

- Postgres 
- RabbitMQ
- ElasticSearch 5.x
- Kibana 5.x
- Prometheus 1.4.x
- Zentral store_worker
- Zentral inventory_worker
- Zentral processor_worker
- Zentral web 
- Nginx

A script is also provided to automatically configure Zentral and Nginx with Let's Encrypt certificates.

*Notes: You must setup DNS with valid FQDN, and set up Firewall with ports 80,443 enabled to succesfully run Let's Encrypt TLS certificate support*

**Amazon AWS**
Go [here](https://github.com/zentralopensource/docs/blob/master/zentral-aws-setup.md) to start a tutorial for AWS based setup.

**Google Cloud**
Go [here](https://github.com/zentralopensource/docs/blob/master/zentral-gcloud-setup.md) to start a tutorial for Google Cloud based setup.

**Vagrant**
Go [here](https://github.com/zentralopensource/docs/blob/master/zentral-vagrant-setup.md) to start a tutorial for VirtualBox / Vagrant based setup.

## SaaS / Managed

 - We build and support SaaS based setups for Zentral for business customers.
 - Details are available on request.

# Support

Custom support available on request. 

- Get in contact via email, [Twitter](<https://twitter.com/zentral_io>) or simply reach out via **#macadmins** Slack channel (subscribe [here](https://macadmins.herokuapp.com)).
- Alternatively you can create an issue with suggestions, questions or requests here on GitHub, and provide your contact details. 



