## Episode 5 - Enroll a OSX device in Terminal from the command line
Of course Zentral needs osquery client to enroll and connect with, this is what we're going to do now.
For Production you will use a LaunchDaemon on all your devices starting osqueryd, but for this tutorial we're fine with
a `--verbose`session in Terminal.


1.) Check if your clients can resolve the Docker hostm, so they will automatically resolve Zentral. If this is **not** the case in your environment then you can edit your `/etc/hosts` file on your mac clients to connect with Zentral.

```bash
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
192.168.94.134  zentral zentral.example.com
```


2.) Check you have osquery installed, open a Terminal window on your OS X client device. 
Use this command:
`osqueryi --version`

```bash
  ## use the shortcut to check for osquery version
  osqueryi --version
  osqueryi version 1.5.3
```

*Note*: 
You may want to download latest osquery .pkg installer here: <https://osquery.io/downloads/>

3.) Copy the `zentral.crt` from the [Zentral-demo-conf] (<https://github.com/zentralopensource/zentral-conf/archive/master.zip>) `tls` directory to your OS X client, for our Tutorial we expect the file in `/Users/Shared` directory.

4.) We need to create a `--database_path` directory for osquery files and it's `RocksDB` database.

```bash
  ## use the shortcut to check for osquery version
  mkdir -p /tmp/zentral/osquery
```
5.) We must create a `enroll_secret.txt` file to make a successful connection from `osqueryd` to the TLS **Zentral/osquery** App.
The osquery `enroll_secret.txt`consist of a shared secret represented in the **base.json** file
We need to bild a simple text file with these ingredients `Shared Secret` and `Computer Serial`

then final file should just contain a strin in this format

```bash
## Pseudo code example
  <your_enroll_secret_here>$SERIAL$<your_device_hardware_serial_here>
```





- Check for your computer serial number

```bash
  /usr/sbin/system_profiler SPHardwareDataType | awk '/Serial/ {print $4}'
``` 

- Create a text file in `/Users/Shared/`

```bash
  touch /Users/Shared/enroll_secret.txt
```

```bash
  ## use your own machine serial in this command
  echo "FOOBARBAZ$SERIAL$C02Q239GHG2" > /Users/Shared/enroll_secret.txt
```

- Verify the text file `/Users/Shared/enroll_secret.txt`

```bash
  cat /Users/Shared/enroll_secret.txt

  FOOBARBAZ$SERIAL$C02Q239GHG2
```


6.) We start the `osqueryd` binary with a lot of arguments, you must change at least one to make it work successfully

- **--tls_hostname** - you must insert your FQDN or IP from the docker host here.

```bash
## this command must be run with sudo, you will need to provide your password 
sudo osqueryd --database_path=/tmp/zentral/osquery --verbose --config_plugin=tls --tls_hostname=zentral.example.com --tls_server_certs=/Users/Shared/zentral.crt --config_tls_endpoint=/osquery/config --config_tls_refresh=60 --enroll_tls_endpoint=/osquery/enroll --enroll_secret_path=/Users/Shared/enroll_secret.txt --logger_tls_endpoint=/osquery/log --logger_tls_period=31 --logger_plugin=tls --distributed_enabled --distributed_plugin=tls --distributed_tls_read_endpoint=/osquery/distributed/read --distributed_tls_write_endpoint=/osquery/distributed/write --distributed_poll_interval=60

```

- The **--verbose** flag should result with output in your Terminal window equal to this example:

```bash
## example 
sudo osqueryd --database_path=/tmp/zentral/osquery --verbose --config_plugin=tls --tls_hostname=zentral.example.com --tls_server_certs=/Users/Shared/zentral.crt --config_tls_endpoint=/osquery/config --config_tls_refresh=60 --enroll_tls_endpoint=/osquery/enroll --enroll_secret_path=/Users/Shared/enroll_secret.txt --logger_tls_endpoint=/osquery/log --logger_tls_period=31 --logger_plugin=tls --distributed_enabled --distributed_plugin=tls --distributed_tls_read_endpoint=/osquery/distributed/read --distributed_tls_write_endpoint=/osquery/distributed/write --distributed_poll_interval=60
I1030 19:01:07.904667 1957675008 init.cpp:273] osquery initialized [version=1.5.3]
I1030 19:01:07.929126 1957675008 processes.cpp:183] An error occurred retrieving the env for pid: 84437
I1030 19:01:07.929256 1957675008 system.cpp:172] Found stale process for osqueryd (84437) removing pidfile
I1030 19:01:07.929486 1957675008 system.cpp:207] Writing osqueryd pid (84476) to /var/osquery/osqueryd.pidfile
I1030 19:01:07.929826 1957675008 extensions.cpp:171] Could not autoload extensions: Failed reading: /etc/osquery/extensions.load
I1030 19:01:07.930681 528384 watcher.cpp:366] osqueryd watcher (84476) executing worker (84477)
I1030 19:01:07.942600 1957675008 init.cpp:270] osquery worker initialized [watcher=84477]
I1030 19:01:07.943572 1957675008 extensions.cpp:178] Could not autoload modules: Failed reading: /etc/osquery/modules.load
I1030 19:01:07.943727 1957675008 db_handle.cpp:124] Opening RocksDB handle: /tmp/zentral/osquery
I1030 19:01:07.950057 1601536 interface.cpp:220] Extension manager service starting: /var/osquery/osquery.em
I1030 19:01:07.950099 1957675008 db_handle.cpp:124] Opening RocksDB handle: /tmp/zentral/osquery
I1030 19:01:07.953806 1957675008 tls.cpp:68] TLSEnrollPlugin requesting a node enroll key from: https://zentral.example.com/osquery/enroll
I1030 19:01:07.954454 1957675008 tls.cpp:196] TLS/HTTPS POST request to URI: https://zentral.example.com/osquery/enroll
I1030 19:01:08.181259 1957675008 tls.cpp:196] TLS/HTTPS POST request to URI: https://zentral.example.com/osquery/config
I1030 19:01:08.426236 1957675008 events.cpp:564] Event publisher failed setup: kernel: Cannot access /dev/osquery
I1030 19:01:08.426491 1957675008 file_access_events.cpp:45] Added kernel listener to: /Users/Shared/
I1030 19:01:08.426697 1957675008 file_events.cpp:58] Added listener to: /Users/Shared/**
I1030 19:01:08.427011 7454720 events.cpp:507] Starting event publisher run loop: diskarbitration
I1030 19:01:08.427014 7991296 events.cpp:507] Starting event publisher run loop: fsevents
I1030 19:01:08.427034 8527872 events.cpp:507] Starting event publisher run loop: iokit_hid
I1030 19:01:08.427048 9064448 events.cpp:507] Starting event publisher run loop: scnetwork
I1030 19:01:08.427284 9601024 tls.cpp:196] TLS/HTTPS POST request to URI: https://zentral.example.com/osquery/distributed/read
```

7.) Wait for a short moment then go to the Zentral > Inventory section and look into the events returned to Zentral: 


- As you know your machine serial can directly enter the events by using a URL like this: 
<https://zentral.example.com/inventory/machine/C02Q239GHG2/events/>


*Note*: 
Kibana and Prometheus are installed with Zentral - we will cover those in another tutorial later.

- To inspect Prometheus the URL would be like: <https://zentral.example.com/prometheus/>
- Kibana requires to **Configure an index pattern** for first use, the URL would be: <https://zentral.example.com/kibana/>




### Expected Result: 
Your Client should enroll with the **Zentral/osquery** App running inside the `zentral/zentral` container on your docker host.
The Probes will download to the client and processed, you will see frequently in terminal how the `osqueryd` binary will contact Zentral via TLS, runs the SQL query and will post back results to Zentral, if Actions trigger in the Probes you should see the in the Zentral User Interface `Inventory > Machine Serial > View Events` Sectio.n
