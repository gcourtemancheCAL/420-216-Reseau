# tftp

**Sommaire**

TFTP (Trivial File Transfer Protocol) est un protocole simple de transfert de fichiers conçu pour être plus léger et plus facile à implémenter que FTP. Il permet le transfert de fichiers entre un client et un serveur. TFTP ne supporter pas l'authentification. TFTP est souvent utilisé pour transférer des fichiers de configuration ou des images de démarrage vers des équipements réseau.

**Protocole de transport:** UDP

**Port:** 69

## Forces et faiblesses

**Forces:**
- Très simple et léger à implémenter
- Faible empreinte mémoire, idéal pour les systèmes embarqués
- Pas besoin d'authentification (utile pour le démarrage réseau)

**Faiblesses:**
- Aucune sécurité ni chiffrement des données
- Pas d'authentification des utilisateurs
- Pas de mécanisme de liste de fichiers (pas de commande `ls`)
- Pas de reprise en cas d'interruption
- Limité aux transferts de fichiers de taille inférieure à 32 Mo 
	- Certaines extensions permettent le transfert de fichier plus gros
- Utilise UDP donc moins fiable que TCP

[RFC1350 - Spécifications de TFTP](https://datatracker.ietf.org/doc/html/rfc1350)

[RFC2347 - Options d'extensions](https://www.rfc-editor.org/rfc/rfc2347.html)

## Cas d'utilisations

- **Démarrage réseau (PXE)** : chargement d'images de système d'exploitation via le réseau
- **Configuration d'équipements réseau** : transfert de fichiers de configuration vers des routeurs, commutateurs
- **Installation de firmware** : installation du firmware ou de l'OS sur des équipements.
- **Sauvegarde/restauration** de configurations d'équipements réseau

