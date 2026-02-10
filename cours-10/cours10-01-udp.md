# UDP et datagramme

## Qu'est-ce que UDP

UDP (User Datagram Protocol) est un protocole de transport (couche 4) simple et sans connexion. Il permet l'envoi de messages appelés datagrammes entre applications sans établir de connexion préalable.

Caractéristiques principales:
- **Sans connexion:** Pas de handshake, pas de session établie
- **Sans garantie:** Aucune garantie de livraison, d'ordre ou de non-duplication
- **Léger:** En-tête minimal (8 octets)
- **Rapide:** Peu de traitement, faible latence

Numéro de protocole IP: **17**

## La datagramme UDP

Le datagramme UDP est composé d'un en-tête simple suivi des données de l'application.

### Structure de l'en-tête UDP

L'en-tête UDP contient seulement 4 champs (8 octets au total):

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

### Champs de l'en-tête

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
- Somme de contrôle pour vérifier l'intégrité
- Optionnel en IPv4, obligatoire en IPv6
- Couvre l'en-tête, les données et un pseudo-en-tête IP

## Fonctionnement de UDP

UDP fonctionne selon un modèle simple d'envoi-réception sans établissement de connexion.

### Processus d'envoi

1. L'application crée un datagramme avec les données à envoyer
2. UDP ajoute son en-tête (ports source/destination, longueur, checksum)
3. Le datagramme est passé à la couche IP pour encapsulation
4. Le paquet IP est transmis sur le réseau

### Processus de réception

1. Le système reçoit un paquet IP avec protocole = 17 (UDP)
2. L'en-tête UDP est analysé
3. Le checksum est vérifié (si présent)
4. Les données sont livrées à l'application qui écoute sur le port destination
5. Si aucune application n'écoute, un message ICMP "Port Unreachable" peut être envoyé

### Aucune gestion d'erreur

UDP ne gère pas:
- **Pertes:** Si un datagramme est perdu, il n'est pas retransmis
- **Ordre:** Les datagrammes peuvent arriver dans le désordre
- **Duplication:** Un datagramme peut être reçu plusieurs fois
- **Contrôle de flux:** Aucun mécanisme pour ralentir l'émetteur

C'est à l'application de gérer ces problèmes si nécessaire.

## Forces et faiblesses

### Forces d'UDP

**1. Vitesse et faible latence:**
- Pas de handshake (connexion immédiate)
- Traitement minimal
- Idéal pour les applications temps réel

**2. Simplicité:**
- En-tête léger (8 octets vs 20+ octets pour TCP)
- Peu de ressources système
- Facile à implémenter

**3. Pas de maintien d'état:**
- Le serveur ne stocke pas d'information sur les clients
- Scalabilité élevée pour de nombreux clients

**4. Broadcast et Multicast:**
- Supporte l'envoi vers plusieurs destinataires simultanément
- TCP ne le permet pas

**5. Contrôle applicatif:**
- L'application décide de la gestion des erreurs
- Flexibilité selon les besoins

### Faiblesses d'UDP

**1. Aucune garantie de livraison:**
- Les paquets peuvent être perdus sans notification
- L'application doit détecter les pertes

**2. Pas de contrôle de congestion:**
- Peut saturer le réseau
- Pas de ralentissement automatique

**3. Désordre possible:**
- Les datagrammes arrivent dans un ordre imprévisible
- L'application doit les réordonner si nécessaire

**4. Pas de protection contre la duplication:**
- Le même datagramme peut arriver plusieurs fois

**5. Sécurité limitée:**
- Facile à spoofier (fausser l'adresse source)
- Vulnérable aux attaques par amplification

## Cas d'utilisation

### Applications appropriées pour UDP

**1. Streaming vidéo/audio:**
- Netflix, YouTube, Spotify
- La perte d'un paquet occasionnel est acceptable
- La latence doit être minimale

**2. Jeux en ligne:**
- Position des joueurs, actions en temps réel
- Données périmées rapidement (pas besoin de retransmission)
- Faible latence critique

**3. VoIP (Voix sur IP):**
- Telephonie, visioconférence (Zoom, Teams)
- Latence plus importante que qualité parfaite
- Données temps réel

**4. DNS (Domain Name System):**
- Requêtes courtes (question/réponse)
- Si pas de réponse, l'application redemande
- Port 53

**5. DHCP (Dynamic Host Configuration Protocol):**
- Attribution d'adresses IP
- Communication broadcast/unicast simple
- Ports 67/68

**6. SNMP (Simple Network Management Protocol):**
- Surveillance réseau
- Interrogations fréquentes et légères
- Port 161/162

**7. Diffusion en direct (live streaming):**
- Événements sportifs, actualités
- Multicast efficace
- Latence minimale

**8. Services de découverte:**
- mDNS (Multicast DNS)
- SSDP (Simple Service Discovery Protocol)
- Broadcast local

### Quand ne PAS utiliser UDP

- Transfert de fichiers (utilisez TCP/FTP/HTTP)
- Transactions financières (fiabilité critique)
- Emails (livraison garantie nécessaire)
- Pages web (HTTP/HTTPS utilise TCP)
- Toute application nécessitant une livraison garantie et ordonnée