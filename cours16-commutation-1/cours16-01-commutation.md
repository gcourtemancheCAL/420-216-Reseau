# Commutation

## Introduction

Le commutateurest un équipement réseau essentiel qui opère principalement au niveau 2 de la pile OSI, soit la couche liaison de données. Son rôle principal est d'interconnecter plusieurs équipements au sein d'un même réseau local (LAN) en créant des connexions temporaires entre les ports pour transférer les trames Ethernet.

## Fonctionnement général 

Le commutateur permet la connexion entre plusieurs systèmes dans un même réseau local. Son rôle consiste à recevoir des trames Ethernet sur un port d'entrée et rediriger ces trames uniquement vers le port approprié.

### Principe de fonctionnement

Le commutateur utilise les **adresses MAC** pour déterminer le bon port vers lequel rediriger une trame. Chaque trame Ethernet contient :
- Une **adresse MAC source** : l'adresse de l'équipement qui envoie la trame
- Une **adresse MAC de destination** : l'adresse de l'équipement qui doit recevoir la trame

Le processus de commutation se déroule en trois étapes principales :

1. Le commutateur examine l'adresse MAC source de chaque trame reçue et l'enregistre dans sa table d'adresses MAC en l'associant au port sur lequel la trame est arrivée
2. Le commutateur consulte sa table pour trouver sur quel port se trouve l'adresse MAC de destination
3. Le commutateur envoie la trame uniquement sur le port correspondant à la destination

## Table d'adresses MAC

La table d'adresse MAC est l'emplacement mémoire interne stockant les associations entre les adresses MAC et les numéros de ports.

Cette structure est constamment mise à jour par le commutateur et c'est elle qui lui permet de déterminer vers quelle port retransmettre une trame.

Le commutateur construit sa table d'adresses MAC automatiquement  :

1. Au démarrage, la table est vide
2. Lorsqu'une trame arrive sur un port, le commutateur lit l'adresse MAC source
3. Il crée ou met à jour une entrée dans sa table associant cette adresse MAC au numéro de port
4. Cette entrée reste valide pendant une durée déterminée
5. Si aucune trame provenant de cette adresse MAC n'est vue pendant ce délai, l'entrée est supprimée

**Exemple de table d'adresses MAC :**

| Adresse MAC          | Port | Âge (secondes) |
|--------------------- |------|----------------|
| 00:1A:2B:3C:4D:5E   | 1    | 45             |
| 00:0C:29:A7:B2:F1   | 2    | 120            |
| 08:00:27:1F:8A:3D   | 3    | 15             |
| AA:BB:CC:DD:EE:FF   | 1    | 200            |

## Diffusion et domaine de diffusion

### Qu'est-ce qu'une diffusion (broadcast) ?

Une **diffusion** est une transmission de trame destinée à tous les équipements d'un réseau - soit adressée à l'adresse de diffusion (Ethernet : **FF:FF:FF:FF:FF:FF** )
### Domaine de diffusion (broadcast domain)

Un **domaine de diffusion** est l'ensemble de tous les équipements qui reçoivent une trame de diffusion émise par n'importe quel membre du groupe.

<img src="img/Pasted image 20260311111123.png" width="800" />

#### Comportement du commutateur avec les diffusions

Lorsqu'une trame en diffusion entre sur un port X du commutateur :
1. Le commutateur reconnaît l'adresse de destination FF:FF:FF:FF:FF:FF
2. Il retransmet cette trame sur **tous les autres ports** (sauf le port X d'origine)
3. Tous les équipements connectés au commutateur reçoivent la trame

##### Exemple 1 : 

**PC0 diffuse un message :**

<img src="img/Pasted image 20260311111438.png" width="800" />

**La trame est reçue par le commutateur :**

<img src="img/Pasted image 20260311111518.png" width="800" />

**Le commutateur retransmet la trame sur les autres ports :**

<img src="img/Pasted image 20260311111619.png" width="800" />

Notez que le routeur va recevoir la trame, mais ne va pas la retransmettre plus loin. Bien que le routeur puisse acheminez les messages vers les autres réseaux, c'est aussi un hôte ordinaire dans son propre réseau. 

##### Exemple 2 : 

**PC4 diffuse un message :**

<img src="img/Pasted image 20260311112027.png" width="800" />

**La trame est reçue par le commutateur :**

<img src="img/Pasted image 20260311112053.png" width="800" />

**Le commuitateur retransmet la trame sur les autres ports :**

<img src="img/Pasted image 20260311112111.png" width="800" />

Remarquez que le routeur est dans deux domaines de diffusion en même temps. Un routeur (de par sa fonction) va toujours être dans **au moins** deux réseaux en même temps. 

#### Comportement lorsque les commutateurs s'enchainent

Ce mécanisme permet d'**enchaîner plusieurs commutateurs ensemble**. Lorsque des commutateurs sont interconnectés :
- Une diffusion émise sur un commutateur se propage à tous les commutateurs connectés
- Tous les équipements sur tous les commutateurs liés reçoivent la diffusion
- On dit que les commutateurs **étendent le domaine de diffusion**

<img src="img/Pasted image 20260311112518.png" width="800" />


##### Enchainement et broadcast

On peut donc voir que les messages diffuser par un système vont traverser tous les commutateurs : 

**PC0 diffuse son message**

<img src="img/Pasted image 20260311112641.png" width="800" />

**La trame est reçue par le premier commutateur

<img src="img/Pasted image 20260311120835.png" width="800" />

**Qui la retransmet sur les autres ports**

<img src="img/Pasted image 20260311120934.png" width="800" />

**Le second commuteur reçoit la trame et complète la procédure**

<img src="img/Pasted image 20260311112814.png" width="800" />

##### Enchainement et unicast

Les associations dans la table d'adresses MAC d'un commutateur se font dans le sens `adresse MAC -> port`. Ainsi, un commutateur peut avoir plusieurs adresses MAC différentes d'associées à un même port. 

Cela permet de supporter une situation où un port est connecté à un autre commutateur.

<img src="img/Pasted image 20260311115854.png" width="800" />

Si l'on assume, dans cette situation, que : 
- Le commutateur A est connecté au port 1 du commutateur B
- L'hôte PC5 est connecté au port 2 du commutateur B
 
Le commutateur B aurait les associations suivantes : 
- `PC0 -> Port 1`
- `PC1 -> Port 1`
- `GW -> Port 1`
- `PC5 -> Port 2`

Ainsi, si PC5 voulait transmettre une trame vers PC0, les étapes suivantes auraient lieux : 

1. PC0 transmet la trame vers le commutateur B.

<img src="img/Pasted image 20260311120455.png" width="800" />

2. Le commutateur B reçoit la trame sur son port 2.

<img src="img/Pasted image 20260311120517.png" width="800" />

3. Le commutateur B chercherait dans sa table pour le port associé au PC0.
4. Le commutateur retransmettrait la trame par son port 1.
5. Le commutateur A recevrait la trame, qui répeterait la maneuvre.

<img src="img/Pasted image 20260311120534.png" width="800" />

6. Au final, le PC0 va recevoir la trame.

<img src="img/Pasted image 20260311120549.png" width="800" />

### Boucle réseau

Un problème peut survenir lorsque des commutateurs sont connectés en boucle. 

Dans des situations de ce genre, une diffusion émise sur un commutateur peut rester prise en transmission pour toujours.

Les équipements modernes vont généralement détecter ces cas de figures et automatiquement désactiver les ports fautifs.

<img src="img/Pasted image 20260311121419.png" width="800" />