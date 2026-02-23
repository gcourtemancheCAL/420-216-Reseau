# TCP et segment

## Qu'est-ce que TCP

TCP (Transmission Control Protocol) est un protocole de transport (couche 4) fiable et orienté connexion. Il fournit des mécanismes permettant la livraison ordonnée des données et gère automatiquement la retransmission des messages lorsque la perte de données est détectée.

Caractéristiques principales:
- **Orienté connexion:** 
	- Établit une session avant l'échange de données
	- Les pairs doivent donc s'annoncer pour que l'échange puisse commencer
- **Fiable:** 
	- Détecte la perte de message et gère la retransmission
	- Fournit des mécanismes garantissant le traitement ordonné des messages.
- **Contrôle de congestion ** 
	- La vitesse de transmission s'ajuste selon la capacité du réseau
	- [Plus de détails](https://en.wikipedia.org/wiki/TCP_congestion_control)

Numéro de protocole IP: **6**

## Le segment TCP

Le segment TCP contient un en-tête complexe (minimum 20 octets) suivi des données de l'application.

### Structure de l'en-tête TCP

<img src="img/Pasted image 20260211093746.png" width="800" />

### Champs principaux

**Port Source / Destination (16 bits chacun):**
- Identifient les applications communicantes

**Numéro de séquence (32 bits):**
- Identifie la position des données dans le flux
- Permet de réordonner les segments
- Valeur initiale aléatoire

**Numéro d'accusé de réception / ACK (32 bits):**
- Indique le prochain octet attendu
- Confirme la réception des données précédentes

**Flags (9 bits) - Les principaux:**
- **SYN:** Synchronisation, établit une connexion
- **ACK:** Accusé de réception valide
- **FIN:** Fermeture de connexion

**Checksum (16 bits):**
- Somme de contrôle obligatoire
- Vérifie l'intégrité de l'en-tête et des données

### La segmentation

La segmentation consiste à séparer un gros message en plus petits segments que l'on envoit indépendemment.

C'est de ce principe que provient le nom de PDU de tcp (le segment tcp).

<img src="img/Pasted image 20260211095134.png" width="800" />

Bien entendu, comme IP ne fournit aucune garantie sur l'ordre de réception, il est possible que les segments arrivent dans le désordre.

<img src="img/Pasted image 20260211095223.png" width="800" />

TCP fournit donc des mécanismes permettant d'assurer l'ordre de réassemblage des segments.

## Fonctionnement de TCP

TCP gère l'ensemble du cycle de vie d'une connexion: établissement, transfert de données, et fermeture.

### TCP handshake

Avant tout échange de données, TCP établit une connexion via un **handshake en 3 étapes** (3-way handshake).

#### Étapes du handshake

<img src="img/Pasted image 20260211093938.png" width="800" />

**1. SYN (Client → Serveur):**
```
Client: "Je veux me connecter"
- Flag SYN = 1
- Numéro de séquence initial = X (aléatoire)
```

**2. SYN/ACK (Serveur → Client):**
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

<img src="img/Pasted image 20260211094326.png" width="800" />

#### Exemple avec perte

```
Émetteur envoie: SEQ 1000, SEQ 1500, SEQ 2000
Récepteur reçoit: SEQ 1000, SEQ 2000 (1500 perdu)
Récepteur envoie: ACK 1500, ACK 1500 (duplicate ACK)
Émetteur détecte: 3 ACK dupliqués = retransmission
```

<img src="img/Pasted image 20260211094730.png" width="800" />

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

<img src="img/Pasted image 20260211094859.png" width="800" />

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

**4. Contrôle de congestion:**
- S'adapte aux conditions du réseau

### Faiblesses de TCP

**1. Overhead élevé:**
- En-tête minimum 20 octets (vs 8 pour UDP)
- Handshake nécessaire (latence initiale)
- ACK pour chaque segment

**2. Latence:**
- Latence plus élevé à cause des mécanismes additionnels de fiabilité.
- Si un segment est perdu, tous les suivants sont bloqués

**3. Pas de multicast:**
- Connexion point-à-point uniquement
- Impossible d'envoyer à plusieurs destinataires

## Cas d'utilisation

**Quelques exemples :**
- http et https
- ftp, sftp
- ssh
- La plupart des protocoles de communication utilisés par les SGBD.
- ...
