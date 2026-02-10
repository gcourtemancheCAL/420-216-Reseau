# TCP et segment

## Qu'est-ce que TCP

TCP (Transmission Control Protocol) est un protocole de transport (couche 4) fiable et orienté connexion. Il garantit la livraison ordonnée et complète des données entre applications.

Caractéristiques principales:
- **Orienté connexion:** Établit une session avant l'échange de données
- **Fiable:** Garantit la livraison, l'ordre et l'intégrité
- **Contrôle de flux:** Ajuste la vitesse selon la capacité du récepteur
- **Contrôle de congestion:** S'adapte aux conditions du réseau
- **Full-duplex:** Communication bidirectionnelle simultanée

Numéro de protocole IP: **6**

## Le segment TCP

Le segment TCP contient un en-tête complexe (minimum 20 octets) suivi des données de l'application.

### Structure de l'en-tête TCP

```
 0                   16                  31
+--------------------+--------------------+
|   Port Source      |  Port Destination  |
+--------------------+--------------------+
|          Numéro de séquence            |
+--------------------+--------------------+
|       Numéro d'accusé de réception     |
+--------------------+--------------------+
| Offset |Rés|Flags |    Fenêtre         |
+--------------------+--------------------+
|    Checksum        | Pointeur urgent    |
+--------------------+--------------------+
|          Options (si présent)          |
+--------------------+--------------------+
|            Données (payload)            |
+--------------------+--------------------+
```

### Champs principaux

**Port Source / Destination (16 bits chacun):**
- Identifient les applications communicantes
- Combinés avec les adresses IP, forment le socket

**Numéro de séquence (32 bits):**
- Identifie la position des données dans le flux
- Permet de réordonner les segments
- Valeur initiale aléatoire (ISN - Initial Sequence Number)

**Numéro d'accusé de réception / ACK (32 bits):**
- Indique le prochain octet attendu
- Confirme la réception des données précédentes

**Data Offset (4 bits):**
- Longueur de l'en-tête TCP en mots de 32 bits
- Minimum 5 (20 octets), maximum 15 (60 octets)

**Flags (9 bits) - Les principaux:**
- **SYN:** Synchronisation, établit une connexion
- **ACK:** Accusé de réception valide
- **FIN:** Fermeture de connexion
- **RST:** Réinitialisation (connexion abandonnée)
- **PSH:** Pousser les données immédiatement
- **URG:** Données urgentes

**Fenêtre (16 bits):**
- Taille du buffer de réception disponible
- Contrôle de flux (combien de données peuvent être envoyées)

**Checksum (16 bits):**
- Somme de contrôle obligatoire
- Vérifie l'intégrité de l'en-tête et des données

**Options (variable):**
- MSS (Maximum Segment Size)
- Window Scaling
- Timestamps
- SACK (Selective Acknowledgment)

## Fonctionnement de TCP

TCP gère l'ensemble du cycle de vie d'une connexion: établissement, transfert de données, et fermeture.

### TCP handshake

Avant tout échange de données, TCP établit une connexion via un **handshake en 3 étapes** (3-way handshake).

#### Étapes du handshake

**1. SYN (Client → Serveur):**
```
Client: "Je veux me connecter"
- Flag SYN = 1
- Numéro de séquence initial = X (aléatoire)
- Port source et destination
```

**2. SYN-ACK (Serveur → Client):**
```
Serveur: "D'accord, voici mon numéro de séquence"
- Flags SYN = 1, ACK = 1
- Numéro de séquence initial = Y (aléatoire)
- ACK = X + 1 (confirme la réception du SYN)
```

**3. ACK (Client → Serveur):**
```
Client: "Connexion établie"
- Flag ACK = 1
- ACK = Y + 1 (confirme la réception du SYN-ACK)
- La connexion est maintenant ESTABLISHED
```

#### Pourquoi 3 étapes?

- **Synchronisation bidirectionnelle:** Chaque côté connaît le numéro de séquence de l'autre
- **Évite les connexions fantômes:** Des paquets retardés ne peuvent pas créer de fausses connexions
- **Sécurité:** Vérifie que les deux parties sont actives

### Numéro de séquence et ack

Les numéros de séquence et d'accusé de réception permettent un transfert fiable.

#### Fonctionnement

**Émetteur:**
```
Segment 1: SEQ = 1000, Longueur = 500 octets, données [1000-1499]
Segment 2: SEQ = 1500, Longueur = 500 octets, données [1500-1999]
Segment 3: SEQ = 2000, Longueur = 500 octets, données [2000-2499]
```

**Récepteur:**
```
Reçoit Segment 1: Envoie ACK = 1500 ("J'attends l'octet 1500")
Reçoit Segment 2: Envoie ACK = 2000
Reçoit Segment 3: Envoie ACK = 2500
```

#### ACK cumulatif

L'ACK confirme tous les octets jusqu'à ce numéro:
- ACK = 2000 signifie "J'ai bien reçu tous les octets jusqu'à 1999"
- Si un segment est perdu, l'ACK reste bloqué

#### Exemple avec perte

```
Émetteur envoie: SEQ 1000, SEQ 1500, SEQ 2000
Récepteur reçoit: SEQ 1000, SEQ 2000 (1500 perdu)
Récepteur envoie: ACK 1500, ACK 1500 (duplicate ACK)
Émetteur détecte: 3 ACK dupliqués = retransmission
```

### Retransmission des segments perdus

TCP détecte et corrige les pertes de segments.

#### Mécanismes de détection

**1. Timeout (RTO - Retransmission Timeout):**
- Un timer démarre à l'envoi de chaque segment
- Si aucun ACK reçu avant expiration, retransmission
- RTO calculé dynamiquement selon le RTT (Round-Trip Time)

**2. Fast Retransmit (Retransmission rapide):**
- Si 3 ACK dupliqués sont reçus, retransmission immédiate
- Plus rapide que d'attendre le timeout

**3. SACK (Selective Acknowledgment):**
- Extension optionnelle de TCP
- Permet d'indiquer précisément quels segments sont reçus
- Évite de retransmettre des segments déjà reçus

#### Processus de retransmission

```
Émetteur:  [SEQ 1000] [SEQ 1500] [SEQ 2000]
              |           X          |
              v                      v
Récepteur:  [ACK 1500]          [ACK 1500]

Émetteur reçoit ACK 1500, puis encore ACK 1500 (duplicate)
Après 3 ACK dupliqués ou timeout:

Émetteur:  [SEQ 1500 - retransmis]
              |
              v
Récepteur:  [ACK 2500] (tous les segments reçus)
```

#### Contrôle de congestion

Après une perte, TCP réduit sa fenêtre d'envoi:
- **Slow Start:** Augmentation exponentielle prudente
- **Congestion Avoidance:** Augmentation linéaire
- **Fast Recovery:** Récupération rapide après perte ponctuelle

### Fermeture de connexion

La fermeture se fait avec un **handshake en 4 étapes**.

```
Client:  [FIN] "J'ai terminé d'envoyer"
            |
            v
Serveur: [ACK] "Je confirme"
         [FIN] "J'ai aussi terminé"
            |
            v
Client:  [ACK] "Connexion fermée"
```

## Forces et faiblesses

### Forces de TCP

**1. Fiabilité:**
- Garantie de livraison de tous les segments
- Détection et retransmission automatiques
- Intégrité vérifiée (checksum)

**2. Ordre garanti:**
- Les données arrivent dans l'ordre d'envoi
- Réassemblage automatique

**3. Contrôle de flux:**
- Le récepteur peut ralentir l'émetteur
- Évite la saturation du buffer

**4. Contrôle de congestion:**
- S'adapte aux conditions du réseau
- Réduit la transmission en cas de congestion
- Équitable entre les flux concurrents

**5. Full-duplex:**
- Communication bidirectionnelle simultanée
- ACK peuvent être piggybackés sur les données

**6. Largement supporté:**
- Implémenté partout
- Bien testé et optimisé

### Faiblesses de TCP

**1. Overhead élevé:**
- En-tête minimum 20 octets (vs 8 pour UDP)
- Handshake nécessaire (latence initiale)
- ACK pour chaque segment

**2. Latence:**
- Handshake de connexion (~ 1 RTT)
- Attente des ACK (buffering)
- Retransmissions en cas de perte

**3. Head-of-Line Blocking:**
- Si un segment est perdu, tous les suivants sont bloqués
- L'application doit attendre la retransmission

**4. Complexité:**
- État de connexion à maintenir (mémoire)
- Nombreux timers et buffers
- Vulnérable aux attaques (SYN flood)

**5. Pas de multicast:**
- Connexion point-à-point uniquement
- Impossible d'envoyer à plusieurs destinataires

**6. Pas optimal pour temps réel:**
- Retransmissions causent des pics de latence
- Données anciennes parfois inutiles

## Cas d'utilisation

### Applications appropriées pour TCP

**1. Navigation web (HTTP/HTTPS):**
- Pages web, images, scripts
- Livraison complète et ordonnée essentielle
- Ports 80, 443

**2. Transfert de fichiers:**
- FTP (ports 20/21)
- SFTP, SCP
- Intégrité critique

**3. Email:**
- SMTP (port 25) - envoi
- POP3 (port 110), IMAP (port 143) - réception
- Aucune perte acceptable

**4. SSH (Secure Shell):**
- Accès distant sécurisé
- Commandes doivent arriver dans l'ordre
- Port 22

**5. Base de données:**
- MySQL (port 3306), PostgreSQL (port 5432)
- Requêtes et réponses doivent être complètes
- Transactions critiques

**6. API REST:**
- Communication client-serveur
- JSON/XML sur HTTP/HTTPS
- Fiabilité nécessaire

**7. Téléchargements:**
- Téléchargement de logiciels, mises à jour
- BitTorrent (peut utiliser TCP)
- Intégrité totale requise

**8. Applications métier:**
- ERP, CRM, comptabilité
- Transactions financières
- Aucune perte tolérée

### Quand privilégier UDP sur TCP

- Streaming en direct (latence > fiabilité)
- Jeux en ligne (données temps réel)
- VoIP (voix périmée inutile)
- DNS (requêtes courtes, retry facile)
- Applications avec leur propre protocole fiable au-dessus d'UDP (QUIC, WebRTC)