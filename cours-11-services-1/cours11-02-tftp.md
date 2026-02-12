# tftp

**Sommaire**

TFTP (Trivial File Transfer Protocol) est un protocole simple de transfert de fichiers conçu pour être plus léger et plus facile à implémenter que FTP. Il permet le transfert de fichiers entre un client et un serveur sans authentification complexe. TFTP est souvent utilisé pour transférer des fichiers de configuration ou des images de démarrage vers des équipements réseau.

**Protocole de transport:** UDP

**Port:** 69

## Forces et faiblesses

**Forces:**
- Très simple et léger à implémenter
- Faible empreinte mémoire, idéal pour les systèmes embarqués
- Pas besoin d'authentification (utile pour le démarrage réseau)
- Rapide pour les petits transferts
- Fonctionne bien dans les environnements PXE (Preboot Execution Environment)
- Consomme peu de bande passante pour l'établissement de connexion

**Faiblesses:**
- Aucune sécurité ni chiffrement des données
- Pas d'authentification des utilisateurs
- Pas de mécanisme de liste de fichiers (pas de commande `ls`)
- Pas de reprise en cas d'interruption
- Limité aux transferts de fichiers de taille inférieure à 32 Mo (avec taille de bloc standard)
- Vulnérable aux attaques par déni de service
- Utilise UDP donc moins fiable que TCP

## Cas d'utilisations

- **Démarrage réseau (PXE)** : chargement d'images de système d'exploitation via le réseau
- **Configuration d'équipements réseau** : transfert de fichiers de configuration vers des routeurs, commutateurs
- **Mises à jour firmware** : installation de nouvelles versions de micrologiciels sur des équipements
- **Sauvegarde/restauration** de configurations d'équipements réseau
- **Environnements de laboratoire** pour des transferts rapides de fichiers
- **Systèmes embarqués** où la simplicité est prioritaire
- **VoIP** : mise à jour de configurations de téléphones IP
