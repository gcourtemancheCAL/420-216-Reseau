# Sockets et ports

## Le concept de port

Un **port** est un identifiant numérique (16 bits) qui permet de distinguer les différentes applications ou services sur un même hôte. Il complète l'adresse IP pour identifier précisément un processus.

### Analogie

Pensez à un immeuble d'appartements:
- **Adresse IP** = Adresse de l'immeuble (ex.: 123 rue Principale)
- **Port** = Numéro d'appartement (ex.: appartement 80)
- **Socket** = Adresse complète (123 rue Principale, app. 80)

### Rôle des ports

Les ports permettent à une machine d'exécuter plusieurs services simultanément:
```
Serveur (192.168.1.10):
- Port 80:  Serveur Web (HTTP)
- Port 443: Serveur Web sécurisé (HTTPS)
- Port 22:  Serveur SSH
- Port 25:  Serveur Email (SMTP)
```

Sans les ports, impossible de faire tourner plusieurs services sur la même adresse IP.

### Valeurs possibles

Les numéros de port sont codés sur **16 bits**, permettant **65 536 valeurs** (0 à 65 535).

#### Répartition des ports

Les ports sont divisés en trois catégories selon l'IANA (Internet Assigned Numbers Authority):

**1. Ports bien connus (Well-Known Ports): 0 - 1023**
- Réservés aux services système standards
- Nécessitent des privilèges administrateur pour être utilisés
- Exemples: 
	- **Protocoles Web:**
		- **Port 80:** HTTP (HyperText Transfer Protocol)
		- **Port 443:** HTTPS (HTTP Secure avec TLS/SSL)
	- **Transfert de fichiers:**
		- **Port 20:** FTP - Données
		- **Port 21:** FTP - Contrôle
		- **Port 22:** SSH/SFTP
	- **Email:**
		- **Port 25:** SMTP
		-  **Port 110:** POP3
		- **Port 143:** IMAP
	- **Services réseau:**
		- **Port 53:** DNS - TCP et UDP
		- **Port 67:** DHCP Server - UDP
		- **Port 68:** DHCP Client - UDP
	- **Accès distant:**
		- **Port 22:** SSH
		- **Port 23:** Telnet

**2. Ports enregistrés (Registered Ports): 1024 - 49 151**
- Enregistrés auprès de l'IANA pour des applications spécifiques
- Peuvent être utilisés par des applications utilisateur
- Exemples: MySQL (3306), PostgreSQL (5432), RDP (3389)

**3. Ports dynamiques/privés (Dynamic/Private Ports): 49 152 - 65 535**
- Ports éphémères assignés automatiquement
- Utilisés comme ports source lors de connexions clientes
- Non enregistrés, libres d'utilisation

## Le socket

Un **socket** est un point de terminaison de communication réseau. Il combine une adresse IP et un port pour identifier de manière unique une connexion.

### Définition formelle

Un socket est défini par un **5-tuple**:
```
(Protocole, IP Source, Port Source, IP Destination, Port Destination)
```

#### Exemple de socket

**Client vers serveur web:**
```
Protocole: TCP
IP Source: 192.168.1.100
Port Source: 54321
IP Destination: 93.184.216.34
Port Destination: 443

Socket: (TCP, 192.168.1.100:54321, 93.184.216.34:443)
```

### Notations

**Format standard:**
```
IPv4: 192.168.1.10:8080
IPv6: [2001:db8::1]:8080
```

Les crochets pour IPv6 évitent l'ambiguïté avec les `:` de l'adresse.

### Types de sockets

**1. Socket TCP (Stream Socket):**
- Orienté connexion
- Flux fiable et ordonné
- Exemple: `SOCK_STREAM` en programmation

**2. Socket UDP (Datagram Socket):**
- Sans connexion
- Datagrammes indépendants
- Exemple: `SOCK_DGRAM` en programmation

**3. Socket Raw:**
- Accès direct aux protocoles bas niveau
- Utilisé pour ping (ICMP), tcpdump, etc.
- Nécessite des privilèges root

### Sockets en programmation

Les applications utilisent des sockets via des APIs système.

#### Cycle de vie d'un socket TCP (serveur)

```
1. socket()     - Créer un socket
2. bind()       - Associer à une adresse/port
3. listen()     - Écouter les connexions
4. accept()     - Accepter une connexion cliente
5. send/recv()  - Échanger des données
6. close()      - Fermer le socket
```

#### Cycle de vie d'un socket TCP (client)

```
1. socket()     - Créer un socket
2. connect()    - Se connecter au serveur
3. send/recv()  - Échanger des données
4. close()      - Fermer le socket
```

### Visualiser les sockets actifs

#### Windows

**Afficher toutes les connexions:**
```cmd
netstat -ano
```
- `-a`: Toutes les connexions et ports en écoute
- `-n`: Adresses numériques (pas de résolution DNS)
- `-o`: Affiche le PID (Process ID)

**Filtrer par protocole:**
```cmd
netstat -ano | findstr TCP
netstat -ano | findstr UDP
netstat -ano | findstr LISTENING
```

#### Linux

**Avec `ss` (recommandé):**
```bash
ss -tuln        # Ports en écoute (TCP + UDP)
ss -tulnp       # Avec les processus (nécessite root)
ss -tan         # Connexions TCP actives
```
- `-t`: TCP
- `-u`: UDP
- `-l`: Listening (en écoute)
- `-n`: Numérique
- `-p`: Processus