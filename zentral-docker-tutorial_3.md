## Episode 3 - Run Zentral in Docker, check your `my-zentral-conf`
To ensure your custom config is working we will use a check procedure

1) Use the Terminal connected to the docker host.
Use the command: docker run -t -i -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral check


- If Check Zentral configuration is running fine you should see the following result. 
No result means your config should be OK, aka the check should not return any error.

```bash
 root@dockerhost:~$ docker run -t -i -v /opt/my-zentral-conf:/home/zentral/conf zentral/zentral check

Check Zentral configuration

```


- If you have an error in your Zentral configuration you may see the following result.
Common errors are usually wrong terminated JSON structs - read more on JSON Structs [here](<http://www.w3resource.com/JSON/introduction.php>)

```bash
root@dockerhost:~$  docker run -t -i -v /opt/my-znetral-conf:/home/zentral/conf zentral/zentral check

Check Zentral configuration

Traceback (most recent call last):
  File "/home/zentral/zentral/bin/check_configuration.py", line 6, in <module>
    zentral.setup()
  File "/home/zentral/zentral/__init__.py", line 9, in setup
    from zentral.conf import settings
  File "/home/zentral/zentral/conf/__init__.py", line 65, in <module>
    settings = load_config_file(find_conf_file(conf_dir, "base"))
  File "/home/zentral/zentral/conf/__init__.py", line 60, in load_config_file
    raise ImproperlyConfigured("{} error in file {}".format(filetype, filepath)) from None
zentral.core.exceptions.ImproperlyConfigured: JSON error in file /home/zentral/conf/base.json

```
*NOTE*: 
You may also try a JSON linter but be extremely careful to post your `base.json` to any online JSON linter - remember you have just inserted some sensible data a moment ago that should not go public in any way (we've warned you here and take no responsibility).


### Expected Result: 
You have successfully run Zentral in Docker(just for a second of course), started a short check for your config and are ready to go on next. 

TIP: As an optional exercise you could have changed the contents of the `tls` folder - we provide some self signed TLS certs but for production you should create your own and best get a 3rd party signed SSL/TLS cert from <https://letsencrypt.org>  (soon) or get a commercial one from [COMODO](<https://ssl.comodo.com/comodo-ssl-certificate.php>) or [VeriSign/Symantec](<https://www.symantec.com/ssl-certificates/>)

