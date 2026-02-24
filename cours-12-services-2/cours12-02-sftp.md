# sftp

## Sommaire

SFTP (SSH File Transfer Protocol) est un protocole sécurisé de transfert de fichiers qui fonctionne sur une connexion SSH existante. Contrairement à FTP, SFTP chiffre l'ensemble de la communication incluant les identifiants et les fichiers transférés. SFTP utilise une seule connexion TCP ce qui le rend plus facile à configurer avec les pare-feu et les NAT.

SFTP est le successeur sécurisé de FTP et offre les mêmes fonctionnalités : téléchargement, téléversement, navigation de répertoires, gestion de fichiers.

**Protocole de transport:** TCP (par-dessus SSH)

**Port:** 22 (port SSH standard)

## Forces et faiblesses

**Forces:**
- Chiffrement fort de toute la communication
- Authentification sécurisée par mot de passe ou par clé publique/privée
- Une seule connexion TCP pour tout le transfert (pas de mode actif/passif)
- Fonctionne bien avec les pare-feu et NAT
- Support complet de la gestion de fichiers (renommer, supprimer, créer des dossiers)
- Navigation dans les répertoires distants
- Reprise en cas d'interruption (selon le client)
- Standard de facto pour le transfert de fichiers sécurisé
- Largement supporté sur tous les systèmes d'exploitation modernes

**Faiblesses:**
- Légèrement plus lourd que FTP en raison du chiffrement
- Nécessite que SSH soit configuré sur le serveur
- Moins rapide que FTP brut en raison du chiffrement (mais la différence est mineure sur les réseaux modernes)

**Note:** SFTP a complètement remplacé FTP et FTPS dans les environnements professionnels et modernes. C'est aujourd'hui le standard pour le transfert de fichiers sécurisé.

## Cas d'utilisations

- **Transfert sécurisé de fichiers** entre clients et serveurs
- **Synchronisation de fichiers** à distance
- **Sauvegarde** de fichiers à distance
- **Distribution de fichiers** volumineux de manière sécurisée
- **Maintenance de serveurs** (téléversement de configuration, scripts, etc.)
- **Partage de fichiers** dans les environnements sensibles (données confidentielles)
