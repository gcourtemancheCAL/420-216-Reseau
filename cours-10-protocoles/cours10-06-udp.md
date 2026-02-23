# UDP et datagramme

## Qu'est-ce que UDP

UDP (User Datagram Protocol) est un protocole de transport (couche 4) simple et sans connexion. Il permet l'envoi de messages appelés datagrammes entre applications sans établir de connexion préalable.

Caractéristiques principales:
- **Sans connexion:** 
	- Pas de handshake, pas de session établie
	- Les pairs peuvent s'ajouter et se retirer sans préavis

<img src="img/Pasted image 20260211084451.png" width="800" />

- **Sans garantie:** 
	- Aucune garantie de livraison
	- Aucune garantie d'ordre 
	- Aucune garantie de non-duplication

- **Peu d'overhead:** 
	- En-tête minimal (8 octets)
	- Peu de traitement, faible latence

Numéro de protocole IP: **17**

## La datagramme UDP

Le datagramme UDP est composé d'un en-tête simple suivi des données de l'application.


```
 0                   16                  31
+--------------------+--------------------+
|   Port Source      |  Port Destination  |
+--------------------+--------------------+
|    Longueur        |     Checksum       |
+--------------------+--------------------+
|                                         |
|            Données (payload)            |
|                                         |
+--------------------+--------------------+
```


**Port Source (16 bits):**
- Identifie le port de l'application émettrice
- Optionnel, peut être 0 si aucune réponse n'est attendue

**Port Destination (16 bits):**
- Identifie le port de l'application réceptrice
- Obligatoire, indique quel service doit recevoir les données

**Longueur (16 bits):**
- Longueur totale du datagramme (en-tête + données)
- Minimum 8 octets (en-tête seul), maximum 65 535 octets

**Checksum (16 bits):**
- Contrôle de vérification de l'intégrité

## Forces et faiblesses

### Forces d'UDP

**1. Vitesse et faible latence:**
- Pas de handshake (connexion immédiate)
- Traitement minimal

**2. Broadcast et Multicast:**
- Supporte l'envoi vers plusieurs destinataires simultanément

**3. Contrôle applicatif:**
- Comme UDP fournit peu (voir pas) de mécanismes de contrôles, l'application est libre de gérer la communication selon ses besoins spécifiques.

### Faiblesses d'UDP

**1. Aucune gestion des erreurs:**
- Les paquets peuvent être perdus sans notification
- Les paquets peuvent arriver dans le désordre
- Les paquets peuvent être dupliqués

**2. Contrôle applicatif:**
- Comme UDP fournit peu (voir pas) de mécanismes de contrôles, l'application DOIT gérer les mécanismes dont elle a besoin.

## Cas d'utilisation

### Applications appropriées pour UDP

**Considérations : **
- Les données doivent arriver rapidement
- On veut minimiser les fluctuations de performances
- Un paquet en retard n'est pas mieux qu'un paquet perdu
- La perte de paquet n'est pas dramatique
- On veut envoyer les messages à plusieurs pairs
- On n'a pas besoin de *connexions*
- L'ordre de réception des messages n'est pas important

**Exemples : **
- Vidéoconférence
- Jeux vidéo en ligne
- DNS
- DHCP
- mDNS
