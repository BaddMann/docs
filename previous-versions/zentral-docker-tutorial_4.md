## Episode 4 - Run Zentral in Docker, in extended Debugging mode
Now we are ready to run Zentral in Docker and link our `my-zentral-config`.
But we don't stop here, we run `docker` with additional options to learn and debug in this tutorial.

1) We start docker with default SSL/TLS Port 443 with the `-p` flag, we link directories on the Docker host with the `-v` flag 
Use this command:
`docker run -t -i -p 443:443 -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral`

2) Start a WebBrowser and enter to your Docker host FQDN or IP address as URL with `https://<yourFQDNhere>/inventory` <https://zentral.excample.com/inventory/> 


3) To enable more options we can run `docker` with additional flags (useful for debugging and inspecting the zentral docker container). 
Use this command:
`docker run -t -i -p 443:443 -v /tmp/supervisorlog:/var/log/supervisor -v /tmp/zentral_notifications:/tmp/zentral_notifications -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral`


```bash
## the result starting docker image should look like this for you
root@dockerhost:~$  docker run -t -i -p 443:443 -v /tmp/supervisorlog:/var/log/supervisor -v /tmp/zentral_notifications:/tmp/zentral_notifications -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral

Check Zentral configuration

Launch supervisor

2015-10-30 10:17:44,534 CRIT Supervisor running as root (no user in config file)
2015-10-30 10:17:44,566 INFO RPC interface 'supervisor' initialized
2015-10-30 10:17:44,566 CRIT Server 'unix_http_server' running without any HTTP authentication checking
2015-10-30 10:17:44,567 INFO supervisord started with pid 7
2015-10-30 10:17:45,572 INFO spawned: 'postgres' with pid 10
2015-10-30 10:17:45,575 INFO spawned: 'server_gunicorn' with pid 11
2015-10-30 10:17:45,579 INFO spawned: 'zentral_store_event_worker' with pid 12
2015-10-30 10:17:45,583 INFO spawned: 'redis' with pid 13
2015-10-30 10:17:45,621 INFO spawned: 'kibana' with pid 16
2015-10-30 10:17:45,626 INFO spawned: 'nginx' with pid 17
2015-10-30 10:17:45,637 INFO spawned: 'prometheus' with pid 18
2015-10-30 10:17:45,655 INFO spawned: 'elasticsearch' with pid 23
2015-10-30 10:17:45,682 INFO spawned: 'zentral_process_event_worker' with pid 25
2015-10-30 10:17:45,715 INFO spawned: 'zentral_inventory_worker' with pid 30
2015-10-30 10:17:45,729 INFO spawned: 'prometheus_push_gateway' with pid 31
2015-10-30 10:17:47,143 INFO success: postgres entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: server_gunicorn entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: zentral_store_event_worker entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: redis entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,143 INFO success: kibana entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: nginx entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: prometheus entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: elasticsearch entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: zentral_process_event_worker entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: zentral_inventory_worker entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
2015-10-30 10:17:47,144 INFO success: prometheus_push_gateway entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)

```

*Note*:
We run the later option 3) as our default here.


### Expected Result: 

You should see the Zentral web interface successfully display your inventory. The key to success here is you must have provided the right API settings to enable Zentral.core connect to `Sal` or your `JSS` Server within the **base.json** .

