# Exercices Packet Tracer - Commutation et domaines de diffusion

## Objectifs

Dans cet exercice, vous allez :
- Créer une topologie réseau simple avec des commutateurs
- Configurer les adresses IP des ordinateurs
- Observer le fonctionnement de la commutation
- Visualiser les trames et le domaine de diffusion
- Comprendre comment les commutateurs apprennent les adresses MAC
- Tester la connectivité entre les équipements

## Partie 1 : Topologie simple avec un commutateur

### 1.1 Création de la topologie

1. Ouvrez Packet Tracer et créez un nouveau projet
2. Ajoutez les équipements suivants à votre espace de travail :
   - **3 ordinateurs (PC)** : Utilisez le modèle PC générique
   - **1 commutateur** : Utilisez le modèle 2960 (ou tout commutateur de base)
1. Connectez les 3 PCs au commutateur.

<img src="img/Pasted image 20260311144958.png" width="600" />

### 1.2 Configuration des adresses IP

Configurez les adresses IP de vos ordinateurs :

**PC0 :** `192.168.1.10/24`
**PC1 :** `192.168.1.11/24`
**PC2 :** `192.168.1.12/24`

### 1.3 Visualisation du processus ARP et de la diffusion

Maintenant, observons ce qui s'est passé lors du premier ping en mode simulation :

1. Passez en **mode Simulation**
2. Effacez les événements précédents en cliquant sur le bouton **Reset Simulation**
3. Dans le panneau de simulation, filtrez pour voir uniquement les protocoles **ICMP** et **ARP**
4. Sur **PC0**, ouvrez à le **Command Prompt** et `pingez` le `192.168.1.12`
5. Utilisez le bouton **Auto Capture / Play** ou avancez étape par étape avec **Capture / Forward**

**Observations à faire :**

1. **Premier paquet - Requête ARP :**
   - Quel est le premier type de paquet envoyé ? (Regardez la colonne "Type")
   - Quelle est l'adresse MAC de destination de la requête ARP ?
   - Vers quels ports/hôtes la requête ARP est transmise? Pourquoi?
   - Pourquoi la requête ARP est-elle envoyée avant le ping ICMP ?

2. **Réponse ARP :**
   - D'où provient la réponse ARP ?
   - Sur quel port du commutateur/vers quel hôte la réponse ARP est-elle transmise ?
   - Pourquoi n'est-elle envoyée que sur un seul port ?

3. **Paquets ICMP :**
   - Combien de paquets ICMP Echo Request sont envoyés ?
   - Le commutateur envoie-t-il les paquets ICMP sur tous les ports ou seulement sur un port spécifique ?
   - Pourquoi le comportement est-il différent entre l'ARP et l'ICMP ?

### 1.4 Observation de la table d'adresses MAC du commutateur

Vérifions maintenant ce que le commutateur a appris :

1. Repassez en **mode Realtime** (icône avec l'éclair)
2. Cliquez sur le **commutateur (Switch0)**
3. Allez dans l'onglet **CLI** (Command Line Interface)
4. Appuyez sur **Entrée** pour activer le terminal
5. Entrez les commandes suivante :
```
enable
show mac address-table
```

**Questions à répondre :**
- Quelles adresses MAC sont listées dans la table ?
- Sur quels ports sont-elles associées ?
- Comment le commutateur a-t-il appris ces adresses ?

### 1.5 Test de diffusion

Maintenant, observons une diffusion avec ping. En mode simulation, à partir du PC0, envoyez un `ping` vers l'adresse de diffusion de votre réseau IP.

Observez comment le commutateur transmet la requête ICMP.

---

## Partie 2 : Extension avec deux commutateurs en série

### 2.1 Ajout d'un second commutateur

Ajoutez les équipements nécessaires de sorte à reproduire cette topologie : 

<img src="img/Pasted image 20260311145325.png" width="600" />

### 2.2 Configuration des adresses IP

Configurez les adresses IP de vos ordinateurs :

**PC3 :** `192.168.1.20/24`
**PC4 :** `192.168.1.21/24`

### 2.3 Observation du chemin des trames à travers deux commutateurs

Passons maintenant en mode simulation pour observer le trajet des trames :

1. Passez en **mode Simulation**
2. **Reset Simulation**
3. Videz les caches ARP de tous les PC (`arp -d` sur chaque PC)
4. Videz les tables MAC des deux commutateurs :
   - Sur chaque commutateur : `clear mac address-table dynamic`
1. Depuis **PC0**, envoyez : `ping 192.168.1.20`
2. Avancez étape par étape et observez

**Observations détaillées :**

1. **Requête ARP initiale :**
   - Où la requête ARP de PC0 est-elle transmise par Switch0 ?
   - Arrive-t-elle jusqu'à Switch1 ?
   - Sur quels ports de Switch1 est-elle transmise ?
   - Quels PC reçoivent finalement cette requête ARP ?
   - Visualisez que **tous les PC du réseau** reçoivent la requête ARP

2. **Réponse ARP :**
   - PC3 répond à la requête ARP
   - Tracez le chemin de la réponse : PC3 → Switch1 → Switch0 → PC0
   - Est-ce que tous les PC reçoivent la réponse ARP ?
   - Pourquoi le comportement est-il différent de la requête ?

3. **Paquets ICMP :**
   - Maintenant que PC0 connaît l'adresse MAC de PC3, observez les paquets ICMP
   - Les requêtes ICMP Echo Request passent-elles par diffusion ou unicast ?
   - Sur quels ports spécifiques les trames sont-elles transmises ?

### 2.5 Observation des tables MAC des deux commutateurs

Après l'échange de paquets, examinons ce que chaque commutateur a appris :

1. Repassez en **mode Realtime**
2. Sur **Switch0**, affichez la table MAC :
```
enable
show mac address-table
```

3. Sur **Switch1**, faites de même

**Questions d'analyse :**

**Pour Switch0 :**
- Quelles adresses MAC sont présentes et sur quels ports ?
- Sur quel port se trouve l'adresse des différents hôtes?
	- PC0, PC1, PC2, PC3, et PC4 ?
- Sur quel port est connecté le deuxième commutateur?
- Que remarquez-vous pour PC3 et PC4?
- Pourquoi les adresses MAC de PC3 et PC4 apparaissent-elles sur le même port de Switch0 ?

**Pour Switch1 :**
- Quelles adresses MAC sont présentes et sur quels ports ?
- Sur quel port se trouve l'adresse MAC de PC0 ?
- Sur quel port se trouve l'adresse MAC de PC1 ?
- Expliquez pourquoi ces adresses apparaissent sur le port qui connecte les deux commutateurs

### 2.6 Visualisation complète du domaine de diffusion

Maintenant, démontrons que tous les équipements font partie du même domaine de diffusion. Envoyez un `ping` vers l'adresse IP de diffusion de votre réseau.

**Observations finales :**

1. **Propagation de la diffusion :**
   - Listez tous les équipements qui reçoivent le paquet de diffusion
   - Le paquet traverse-t-il les deux commutateurs ?

2. **Comportement des commutateurs :**
   - Switch0 envoie-t-il la diffusion sur tous ses ports ?
   - Switch1 reçoit-il la diffusion ?
   - Switch1 retransmet-il la diffusion sur tous ses ports (sauf celui d'où elle vient) ?

3. **Conclusion sur le domaine de diffusion :**
   - Combien d'équipements (PC) sont dans le même domaine de diffusion ?
   - Les deux commutateurs forment-ils un seul grand domaine de diffusion ou deux domaines séparés ?
   - Expliquez comment les commutateurs "étendent" le domaine de diffusion