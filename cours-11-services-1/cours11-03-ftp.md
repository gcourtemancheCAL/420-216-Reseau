# ftp

**Sommaire**

FTP (File Transfer Protocol) est un protocole standard de transfert de fichiers sur un réseau TCP/IP. Il permet de transférer des fichiers entre un client et un serveur, de naviguer dans les répertoires distants, et de gérer des fichiers (renommer, supprimer, créer des dossiers). FTP utilise deux connexions : une pour les commandes (canal de contrôle) et une pour les données (canal de données).

**Protocole de transport:** TCP

**Port:** 21 (contrôle), 20 (données en mode actif)

## Mode actif et passif

### Mode actif

1. Le client se connecte au port de contrôle du serveur (21) à partir d'un port aléatoire.
2. Le client ftp envoie une commande `PORT` au serveur, indiquant sur quel port se connecté pour effectuer le transfert de données.
3. Le serveur crée un nouvelle connexion avec le client en se connectant au port qui lui a été fourni à partir de son port 20.

Le mode actif peine à traverser les pare-feu et les NATs.

### Mode passif

1. Le client se connecte au port de contrôle du serveur (21) à partir d'un port aléatoire.
2. Le client ftp envoie une commande `PASV` au serveur. 
3. Le serveur répond avec un port sur lequel établir la connexion de données
4. Le client se connecte à ce port.

Comme le client initie la connexion pour le transfert de données, ce mode fonctionne mieux pour traverser les pare-feu et les NATs.

## Forces et faiblesses

**Forces:**
- Transfert fiable de fichiers grâce à TCP
- Supporte les transferts de fichiers volumineux
- Permet la reprise de transferts interrompus
- Navigation dans les répertoires distants
- Gestion complète des fichiers (renommer, supprimer, etc.)

**Faiblesses:**
- Aucun chiffrement par défaut (FTP standard)
- Authentification en texte clair (nom d'utilisateur et mot de passe)
- Complexité de configuration avec les pare-feu (deux connexions)
- Mode actif peut poser problème avec NAT
- Spécifications très vague, complexe et contradictoire

**Note:** FTPS (FTP Secure) et SFTP (SSH File Transfer Protocol) sont des alternatives sécurisées. SFTP a principalement remplacé FTP et FTPS.

## Cas d'utilisations

- **Partage de fichiers volumineux** au sein d'une organisation
- **Distribution de logiciels** et de mises à jour
- **Transfert de fichiers entre serveurs**
- **Archives publiques** de fichiers (miroirs de logiciels)
- **Transfert automatisé** de fichiers entre systèmes (scripts)
