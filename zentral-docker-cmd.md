## Working with the zentral configuration and probes

To change the `zentral` configuration or probes, edit the files in `/home/zentral/conf` on the server. When you are done, go in the `/home/zentral/zentral` dir (where the `docker-compose.yml` file is) and restart the updated services.

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
