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
then using your favorite text edit createe ```override.conf```



