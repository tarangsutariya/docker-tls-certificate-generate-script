# docker-tls-certificate-generate-script
A shell script to generate tls certificate to be used for securing docke daemon


to run the script

```
chmod +x generate.sh
./generate.sh
```

then provide signing password as well as domain name what will be used to connect to docker daemon remotly

it creates required files in .docker directory 

```
root@de:~# ls .docker
ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
```
