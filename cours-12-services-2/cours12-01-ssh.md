# ssh

## Sommaire

SSH (Secure Shell) est un protocole réseau sécurisé qui permet d'établir une connexion chiffrée vers un serveur distant. Contrairement à Telnet qui transmet les données en texte clair, SSH chiffre l'ensemble de la communication, y compris les identifiants de connexion. SSH permet l'accès à une interface shell distante de manière sécurisée.

SSH fonctionne selon un modèle client-serveur où le client SSH envoie des commandes au serveur SSH (sshd) et reçoit les résultats de manière chiffrée.

**Protocole de transport:** TCP

**Port:** 22

## Forces et faiblesses

**Forces:**
- Chiffrement fort de toute la communication (clés publique/privée)
- Authentification sécurisée par mot de passe ou par clé
- Protection contre les attaques man-in-the-middle
- Fonction de tunneling (port forwarding)
- Support de X11 forwarding pour les applications graphiques
- Standard de facto pour l'accès à distance sécurisé
- Largement supporté sur tous les systèmes d'exploitation modernes

**Faiblesses:**
- Légèrement plus "lourd" à l'utilisation que Telnet en raison du chiffrement
- Nécessite une configuration appropriée de la clé publique/privée
- Peut être vulnérable à une mauvaise configuration (ex: connexion root autorisée)

**Note:** SSH a complètement remplacé Telnet dans les environnements professionnels et modernes. C'est aujourd'hui le standard incontournable pour l'accès à distance sécurisé.

## Cas d'utilisations

- **Administration distante** de serveurs Linux/Unix
- **Accès sécurisé** à un terminal distant
- **Transfert de fichiers sécurisé** (via SFTP)
- **Tunneling** et forwarding de ports
- **Exécution de commandes** à distance de manière automatisée
- **Branchement sécurisé** sur des machines en production
