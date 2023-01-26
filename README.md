# docker-tls-certificate-generate-script
A shell script to generate tls certificate to be used for securing docke daemon


to run the script

```
wget https://raw.githubusercontent.com/tarangsutariya/docker-tls-certificate-generate-script/main/generate.sh
chmod +x generate.sh
./generate.sh
```

then when prompted provide signing password as well as domain name what will be used to connect to docker daemon remotly

it creates required files in .docker directory 

```
root@de:~# ls .docker
ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
```

then , to enable tls-verification in docker

```
mkdir /etc/systemd/system/docker.service.d
```
then using your favorite text edit create
```/etc/systemd/system/docker.service.d/override.conf```
with contents

```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D -H unix:///var/run/docker.sock --tlsverify --tlscert=/root/.docker/server-cert.pem --tlscacert=/root/.docker/ca.pem --tlskey=/root/.docker/server-key.pem -H tcp://0.0.0.0:2376
```

change the path to match your setup and change tcp://0.0.0.0:2376 with host and ip you will use to connect to docker daemon

Restart docker service
```
sudo systemctl restart docker
sudo systemctl status docker
```
if everything worked you should see something like

```
 docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset>
    Drop-In: /etc/systemd/system/docker.service.d
             └─override.conf
     Active: active (running) since Thu 2023-01-26 11:56:16 IST; 1h 22min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 20907 (dockerd)
      Tasks: 10
     Memory: 34.3M
        CPU: 921ms
     CGroup: /system.slice/docker.service
             └─20907 /usr/bin/dockerd -D -H unix:///var/run/docker.sock --tlsv
  ```
  
 to connect docker daemon from remote machine copy ca.pem,cert.pem and key.pem to remote machine and use something like
 ```
 docker --tlsverify \
    --tlscacert=ca.pem \
    --tlscert=cert.pem \
    --tlskey=key.pem \
    -H=$HOST:2376 version
    ```
    to connect



