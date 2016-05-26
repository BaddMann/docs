## Episode 6 - Enter the Docker host for Debugging.

In case things don't work as expected you may want to find the bug.
So here is a short but (hopefully) usefull debugging session to help you with some examples and logfiles to look into.

#### Docker host debug with logfiles:

1) In a new Terminal connect to your docker host, we want look into the debugging options now.
You may find errors in these areas:

- **Performance** or startup errors - check the:  `server_gunicorn-stderr---supervisor--XXXXX.log` file
- **inventory** connection problems - check the:  `zentral_inventory_worker-stderr---supervisor-XXXXX.log` file
- **Action** or processing errors - check the:  `zentral_process_event_worker-stderr---supervisor-XXXXX.log` file
- **TLS** or Nginx Web server errors - check the:  `nginx-stderr---supervisor-XXXXX.log` file


```bash
root@dockerhost:~$ ls supervisorlog/
elasticsearch-stderr---supervisor-9p9VYn.log
elasticsearch-stdout---supervisor-WqLMIF.log
kibana-stderr---supervisor-pY_lZ2.log
kibana-stdout---supervisor-Xwltxo.log
nginx-stderr---supervisor-YFF5JY.log
nginx-stdout---supervisor-cppOu4.log
postgres-stderr---supervisor-qxyqfN.log
postgres-stdout---supervisor-Mcbb3U.log
prometheus-stderr---supervisor-nZi0m5.log
prometheus-stdout---supervisor-jCxJt2.log
prometheus_push_gateway-stderr---supervisor-f6BaVt.log
prometheus_push_gateway-stdout---supervisor-Cn5gYt.log
redis-stderr---supervisor-77SVQa.log
redis-stdout---supervisor-rE0JAR.log
server_gunicorn-stderr---supervisor-OqX1RS.log
server_gunicorn-stdout---supervisor-PHLrxz.log
supervisord.log
zentral_inventory_worker-stderr---supervisor-KArl02.log
zentral_inventory_worker-stdout---supervisor-ALsEUe.log
zentral_process_event_worker-stderr---supervisor-n6DeOH.log
zentral_process_event_worker-stdout---supervisor-6E5qEO.log
zentral_store_event_worker-stderr---supervisor-YJOi61.log
zentral_store_event_worker-stdout---supervisor-d36tPm.log
```

2) The next debugging technique is one targeted onActions, by default in base.json you'll find `debug` option setting activated.
Once an Probe `Action` triggers a timestamped JSON file will be written to the `local_dir`.


```bash
  "actions": {
    "debug": {
      "backend": "zentral.core.actions.backends.json_file",
      "local_dir": "/tmp/zentral_notifications/"
    },
```

#### Example debug case:
You have setup a Slack webhook in your **base.json** but no info happens to be posted to the Slack channel, how to find out if at least the Probe Actions were successfull?

Connect tom the Docker host and consult the `local_dir` (defaults to `/tmp/zentral_notifications/`). 
See if you find some matching subdirectories and time-stampted files inside. 
Look inside the files (using the `cat` unix tool), if so your Probes may work fine but the Slack API/webhook should be double checked (with `curl` like provided in the [Slack API doku](<https://api.slack.com/incoming-webhooks>) ). 


- Empty debug directory looks like this:

```bash
root@dockerhost:~$ ls -la /tmp/zentral_notifications/
total 8
drwxr-xr-x 2 root root 4096 Oct 30 11:15 .
drwxrwxrwt 9 root root 4096 Oct 30 11:42 ..
```

- Inspect a the debug directory like this:

```bash
root@dockerhost:~$ ls -la /tmp/zentral_notifications/
drwxr-xr-x 5 root root 4096 Oct 30 12:01 .
drwxrwxrwt 9 root root 4096 Oct 30 11:42 ..
drwxr-xr-x 2 root root 4096 Oct 30 12:01 dropbox_alert
drwxr-xr-x 2 root root 4096 Oct 30 12:01 little_snitch
drwxr-xr-x 2 root root 4096 Oct 30 12:01 osx-attacks

root@dockerhost:~$ ls -la /tmp/zentral_notifications/dropbox_alert/
total 12
drwxr-xr-x 2 root root 4096 Oct 30 12:01 .
drwxr-xr-x 6 root root 4096 Oct 30 12:02 ..
-rw-r--r-- 1 root root  429 Oct 30 12:01 2015-10-30T11:01:05.986639

root@dockerhost:~$ cat /tmp/zentral_notifications/dropbox_alert/2015-10-30T11\:01\:05.986639 
{
    "body": "Name: mb-sal\nSerial number: C02Q239GHG2\n\nModel: Apple, MacBook (12-inch Retina Early 2015)\nProcessor: Intel Core M 1.2GHZ\nTotal RAM: 8.0 GiB\nFileVault 2: not encrypted\nView in jss: https://jss.example.com:8443/computers.html?id=52&o=r\nZentral event id: 0538753a-2b99-466c-b20f-6c31d5cf9a66\n\naction: added\nname: Dropbox\npid: 586\nport: 17500\n",
    "subject": "Zentral notification - dropbox_alert"

```

### Expected Result: 
You should see debug files for Notifications in `/tmp/zentral_notifications/` directory. In case of errors you know the logfiles to look into and provide them as feedback for bug reports.
