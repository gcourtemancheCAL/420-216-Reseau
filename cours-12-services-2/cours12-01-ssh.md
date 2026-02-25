# ssh

## Sommaire

SSH (Secure Shell) est un protocole réseau sécurisé qui permet d'établir une connexion chiffrée vers un serveur distant. Les commandes envoyées par le client sont exécutées sur le serveur qui lui retourne le résultat. L'entièreté de la communication est chiffrée. 

Contrairement à Telnet qui transmet les données en texte clair, SSH chiffre l'ensemble de la communication, y compris les identifiants de connexion. SSH permet l'accès à une interface shell distante de manière sécurisée.

**Protocole de transport:** TCP

**Port:** 22

## Forces et faiblesses

**Forces:**
- Chiffrement fort de toute la communication (clés publique/privée)
- Authentification sécurisée par mot de passe ou par clé
- Standard de facto pour l'accès à distance sécurisé
- Largement supporté sur tous les systèmes d'exploitation modernes

**Note:** SSH a remplacé Telnet dans la plupart des contextes.
