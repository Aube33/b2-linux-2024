# I. Init
## 1. Installation de Docker 
## 2. Vérifier que Docker est bien là
### 3. sudo c pa bo
#### 🌞 Ajouter votre utilisateur au groupe docker

### 4. Un premier conteneur en vif
#### 🌞 Lancer un conteneur NGINX
```
aube@MakOS:~$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
57b64962dd94: Download complete 
8cc1569e58f5: Download complete 
13e320bf29cd: Download complete 
bc0965b23a04: Download complete 
650ee30bbe5e: Download complete 
362f35df001b: Download complete 
7b50399908e1: Download complete 
Digest: sha256:fb197595ebe76b9c0c14ab68159fd3c08bd067ec62300583543f0ebda353b5be
Status: Downloaded newer image for nginx:latest
ddd2d70a1f313576ea5728044ea6d440fe91b5932ffdbb6d5eb83e805f95be02
```
#### 🌞 Visitons
- vérifier que le conteneur est actif avec une commande qui liste les conteneurs en cours de fonctionnement:
```
aube@MakOS:~$ docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED         STATUS          PORTS                    NAMES
ddd2d70a1f31   nginx                           "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes    0.0.0.0:9999->80/tcp     musing_dewdney
```
- afficher les logs du conteneur
```
aube@MakOS:~$ docker logs musing_dewdney
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/12/11 09:08:22 [notice] 1#1: using the "epoll" event method
2024/12/11 09:08:22 [notice] 1#1: nginx/1.27.3
2024/12/11 09:08:22 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/12/11 09:08:22 [notice] 1#1: OS: Linux 6.10.14-linuxkit
2024/12/11 09:08:22 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/12/11 09:08:22 [notice] 1#1: start worker processes
2024/12/11 09:08:22 [notice] 1#1: start worker process 29
2024/12/11 09:08:22 [notice] 1#1: start worker process 30
2024/12/11 09:08:22 [notice] 1#1: start worker process 31
2024/12/11 09:08:22 [notice] 1#1: start worker process 32
2024/12/11 09:08:22 [notice] 1#1: start worker process 33
2024/12/11 09:08:22 [notice] 1#1: start worker process 34
2024/12/11 09:08:22 [notice] 1#1: start worker process 35
2024/12/11 09:08:22 [notice] 1#1: start worker process 36
```
- afficher toutes les informations relatives au conteneur avec une commande docker inspect
```
aube@MakOS:~$ docker inspect musing_dewdney | head -n 20
[
    {
        "Id": "ddd2d70a1f313576ea5728044ea6d440fe91b5932ffdbb6d5eb83e805f95be02",
        "Created": "2024-12-11T09:08:21.723730641Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1105,
            "ExitCode": 0,
            "Error": "",
```
- afficher le port en écoute sur la VM avec un sudo ss -lnpt:
```
aube@MakOS:~$ sudo ss -lntp | grep 9999
LISTEN 0      4096               *:9999             *:*    users:(("com.docker.back",pid=1702,fd=86)) 
```
- ouvrir le port 9999/tcp (vu dans le ss au dessus normalement) dans le firewall de la VM:
```
tkt
```
- depuis le navigateur de votre PC, visiter le site web sur http://IP_VM:9999:
```
aube@MakOS:~$ curl localhost:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### 🌞 On va ajouter un site Web au conteneur NGINX
```
aube@MakOS:~/temp/nginx$ docker run -d -p 9999:8080 -v /home/aube/temp/nginx/index.html:/var/www/html/index.html -v /home/aube/temp/nginx/site_nul.conf:/etc/nginx/conf.d/site_nul.conf nginx
fde6d177950ef12392a885c76838806f416e7c14eb034cf1f43d68775dab58fc
```

#### 🌞 Visitons
- vérifier que le conteneur est actif:
```
aube@MakOS:~/temp/nginx$ docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED              STATUS              PORTS                            NAMES
fde6d177950e   nginx                           "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp, 0.0.0.0:9999->8080/tcp   compassionate_einstein
```
- visiter le site web depuis votre PC:
```
aube@MakOS:~/temp/nginx$ curl localhost:9999
<h1>MEOOOW</h1>
```

### 5. Un deuxième conteneur en vif
#### 🌞 Lance un conteneur Python, avec un shell
```
aube@MakOS:~/temp/nginx$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
2ac2596c631f: Download complete 
551df7f94f9c: Download complete 
5f0e19c475d6: Download complete 
ce82e98d553d: Download complete 
fdf894e782a2: Download complete 
5bd71677db44: Download complete 
abab87fa45d0: Download complete 
Digest: sha256:220d07595f288567bbf07883576f6591dad77d824dce74f0c73850e129fa1f46
Status: Downloaded newer image for python:latest
root@6e355b1260f8:/# 
```
#### 🌞 Installe des libs Python
```
root@6e355b1260f8:/# pip install aiohttp aioconsole
Collecting aiohttp
  Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (7.7 kB)
Collecting aioconsole
  Downloading aioconsole-0.8.1-py3-none-any.whl.metadata (46 kB)
Collecting aiohappyeyeballs>=2.3.0 (from aiohttp)
  Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl.metadata (6.1 kB)
Collecting aiosignal>=1.1.2 (from aiohttp)
...

root@6e355b1260f8:/# python
Python 3.13.1 (main, Dec  4 2024, 20:40:27) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
>>> exit()

root@6e355b1260f8:/# ls
bin  boot  dev	etc  home  lib	lib64  media  mnt  opt	proc  root  run  sbin  srv  sys  tmp  usr  var
root@6e355b1260f8:/# ip a
bash: ip: command not found
```