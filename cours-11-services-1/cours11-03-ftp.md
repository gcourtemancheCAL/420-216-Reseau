# ftp

**Sommaire**

FTP (File Transfer Protocol) est un protocole standard de transfert de fichiers sur un réseau TCP/IP. Il permet de transférer des fichiers entre un client et un serveur, de naviguer dans les répertoires distants, et de gérer des fichiers (renommer, supprimer, créer des dossiers). FTP utilise deux connexions : une pour les commandes (canal de contrôle) et une pour les données (canal de données).

**Protocole de transport:** TCP

**Port:** 21 (contrôle), 20 (données en mode actif)

## Forces et faiblesses

**Forces:**
- Transfert fiable de fichiers grâce à TCP
- Supporte les transferts de fichiers volumineux
- Permet la reprise de transferts interrompus
- Navigation dans les répertoires distants
- Gestion complète des fichiers (renommer, supprimer, etc.)
- Deux modes de transfert : ASCII et binaire
- Modes actif et passif pour contourner les pare-feu
- Largement supporté par tous les systèmes d'exploitation

**Faiblesses:**
- Aucun chiffrement par défaut (FTP standard)
- Authentification en texte clair (nom d'utilisateur et mot de passe)
- Complexité de configuration avec les pare-feu (deux connexions)
- Vulnérable aux attaques par interception
- Pas adapté aux réseaux non sécurisés
- Mode actif peut poser problème avec NAT

**Note:** FTPS (FTP Secure) et SFTP (SSH File Transfer Protocol) sont des alternatives sécurisées.

## Cas d'utilisations

- **Hébergement web** : transfert de fichiers vers des serveurs web
- **Partage de fichiers volumineux** au sein d'une organisation
- **Sauvegarde de données** vers des serveurs distants
- **Distribution de logiciels** et de mises à jour
- **Transfert de fichiers entre serveurs**
- **Archives publiques** de fichiers (miroirs de logiciels)
- **Gestion de contenu** pour les sites web
- **Transfert automatisé** de fichiers entre systèmes (scripts)
- **Synchronisation de données** entre sites distants
