## Episode 1 - Start to install Zentral in Docker

You need to have some Terminal skills to get started with Docker. 
We will pull the `zentral/zentral` Docker image from Docker Hub in this Episode.

#### Pull the `zentral/zentral` image from Docker Hub

1.) Find out the IP of your Docker host
2.) Use Terminal to connect with `ssh` to your Docker host

```bash
## NOTE: your IP will be different to the one we use below.
## connect as user root to a Docker host with IP 192.168.56.10

ssh root@192.168.56.10
```

3.) Run a short Docker version check in Terminal 
Use the command: `docker --version`

```bash
  root@dockerhost:~$ docker --version
  Docker version 1.8.3, build f4bf5c7
```

4.) Pull the zentral/zentral Docker image from Docker Hub, this process will take a while (5-20min depending on your d/l speed)
Use the command: `docker pull zentral/zentral`

```bash
root@dockerhost:~$ docker pull zentral/zentral
Using default tag: latest
latest: Pulling from zentral/zentral

3fd0c2ae8ed2: Pulling fs layer 
9e19ac89d27c: Pulling fs layer 
ac65c371c3a5: Pulling fs layer
... 
... # snipped out lengthy strings here
...
4d5ef7a0612a: Pull complete 
aefa3f7e9fea: Pull complete 
Digest: sha256:08c26f427cf04dbc95e30c885f1d6c138c469b40962763b21fc215e43cb57e78
Status: Downloaded newer image for zentral/zentral:latest
```

### Expected Result: 
You have managed to download `zentral/zentral` image from Docker Hub with the `docker pull` command.
