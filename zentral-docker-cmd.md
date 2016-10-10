## Working with the Zentral configuration and setup

In **docker-compose** based setups:

To change the `zentral` configuration, edit files in `/home/zentral/conf` on the server. Mainly you may edit the following:

- `base.json` - edit here for the main configuration like adding actions, integrations, etc. 
- `contacts.json` edit here for contacts of admins, admin-groups to notify via email, SMS
- `zentral.htpasswd` edit here for change the password htaccess protected Zentral web interface, Kibana, Prometheus
- `nginx/conf.d/zentral.conf` edit here for nginx based settings
- `tls` in this directory exchange certificate files with your own full chain of 3rd party signed TLS/SSL certs

When you are done with editing, go to `/home/zentral/zentral` dir (where `docker-compose.yml` file is located) and restart the updated services with `docker-compose` .

## Commands for docker-compose

To start everything :

```bash
$ sudo docker-compose up -d
```

For a complete restart of `zentral` :

```bash
$ sudo docker-compose restart inventory_worker \
                              processor_worker \
                              store_worker \
                              web
```

Web server code update:

```bash
$ sudo docker-compose pull web && \
  sudo docker-compose up -d --no-deps web
```

Complete update:

```bash
$ sudo docker-compose pull web && \
  sudo docker-compose up -d --no-deps inventory_worker \
                                      processor_worker \
                                      store_worker \
                                      web
```

List containers:

```bash
$ sudo docker-compose ps
```

Follow web logs:

```bash
$ sudo docker-compose logs web
```

Follow nginx logs:

```bash
$ sudo docker-compose logs nginx
```

