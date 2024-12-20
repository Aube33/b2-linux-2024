# TP2 : Utilisation courante de Docker
## I. Commun à tous : App Web
### 0. Setup
### I. Packaging de l'app PHP
#### 🌞 docker-compose.yml

# TP2 dév : packaging et environnement de dév local
## I. Packaging
### 1. Calculatrice
#### 🌞 Le lien vers ton dépôt
Lien du repo: https://github.com/Aube33/b2-linux-2024-calculatrice

### 2. Chat room
#### 🌞 Packager l'application de chat room
Lien du repo: https://github.com/Aube33/b2-linux-2024-chatroom

### 3. Ur own
#### 🌞 Packager une application à vous
Lien du repo: https://github.com/Aube33/b2-linux-2024-websitemulti

(Le code est vieux et dégeulasse merci de ne pas y prêter attention)

# TP2 admins : Web stack
## I. Good practices
#### 🌞 Limiter l'accès aux ressources
On verra plus tard ça 

# Bonus CI/CD
## 0. Setup
## I. Premiers pas CI
### 1. Préparation runner
#### 🌞 Préparer un fichier de conf pour le Runner
#### 🌞 Lancer le Runner
### 2. Une première pipeline
#### 🌞 Créer un fichier .gitlab-ci.yml
## II. Premier déploiement 
### 1. Préparation
#### A. SSH
#### 🌞 Générez une nouvelle paire de clés SSH
#### 🌞 Déposer la clé publique sur prod.bonus
#### B. Gitlab
#### 🌞 Créer une variable de CI qui contient la clé privée
### 2. Déploiement automatique : CD
#### 🌞 Adaptez votre .gitlab-ci.yml
## III. Cas concret ?
Lien du repo: https://gitlab.com/aubegroupe/b2-linux-2024-bonus/

# TP3 Sécu : Linux Hardening
## 0. Setup
## 1. Guides CIS
### 🌞 Suivre un guide CIS

- la section 2.1 ✅
- les sections 3.1 3.2 et 3.3 ✅
- toute la section 5.2 Configure SSH Server ✅
- au moins 10 points dans la section 6.1 System File Permissions ✅
- au moins 10 points ailleur sur un truc que vous trouvez utile (1.3.1) ✅

## 2. Conf SSH
### 🌞 Chiffrement fort côté serveur
https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process
https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html

- conf dans le fichier de conf
- regénérer des clés pour le serveur ?

`$ sudo ssh-keygen -A`

- regénérer les paramètres Diffie-Hellman ? (se renseigner sur Diffie-Hellman ?)

### 🌞 Clés de chiffrement fortes pour le client
https://tutox.fr/2020/04/16/generer-des-cles-ssh-qui-tiennent-la-route/

`$ ssh-keygen -t ed25519`
```
aube@MakOS:~/Downloads$ ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 2222 antoine@10.0.1.10
/sbin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/aube/.ssh/id_ed25519.pub"
/sbin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/sbin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
Enter passphrase for key '/home/aube/.ssh/id_rsa': 

Number of key(s) added: 1

Now try logging into the machine, with: "ssh -i /home/aube/.ssh/id_ed25519 -p 2222 'antoine@10.0.1.10'"
and check to make sure that only the key(s) you wanted were added.
```

### 🌞 Connectez-vous en SSH à votre VM avec cette paire de clés
```
aube@MakOS:~/Downloads$ ssh -vvvv -i /home/aube/.ssh/id_ed25519 -p 2222 'antoine@10.0.1.10'
OpenSSH_9.9p1, OpenSSL 3.4.0 22 Oct 2024
debug1: Reading configuration data /etc/ssh/ssh_config
debug3: /etc/ssh/ssh_config line 2: Including file /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf depth 0
debug1: Reading configuration data /etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf
debug2: resolve_canonicalize: hostname 10.0.1.10 is address
debug3: expanded UserKnownHostsFile '~/.ssh/known_hosts' -> '/home/aube/.ssh/known_hosts'
debug3: expanded UserKnownHostsFile '~/.ssh/known_hosts2' -> '/home/aube/.ssh/known_hosts2'
debug3: channel_clear_timeouts: clearing
debug3: ssh_connect_direct: entering
debug1: Connecting to 10.0.1.10 [10.0.1.10] port 2222.
debug3: set_sock_tos: set socket 3 IP_TOS 0x48
debug1: Connection established.
debug1: identity file /home/aube/.ssh/id_ed25519 type 3
debug1: identity file /home/aube/.ssh/id_ed25519-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_9.9
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.7
debug1: compat_banner: match: OpenSSH_8.7 pat OpenSSH* compat 0x04000000
debug2: fd 3 setting O_NONBLOCK
debug1: Authenticating to 10.0.1.10:2222 as 'antoine'
debug3: put_host_port: [10.0.1.10]:2222
```

## 4. DoT
### 🌞 Configurer la machine pour qu'elle fasse du DoT
### 🌞 Prouvez que les requêtes DNS effectuées par la machine...