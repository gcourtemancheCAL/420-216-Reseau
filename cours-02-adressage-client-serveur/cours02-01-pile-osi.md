# Modèles de fonctionnement du réseau
## Pile OSI

<img src="img/Pasted image 20260114104806.png" width="500" />

## Modèle TCP/IP

<img src="img/Pasted image 20260114105140.png" width="700" />

## PDU

Unité de données échangés entre paires au travers un protocole spécifique.
## Description des couches

### Couches 7-6-5

Ces couches représentent les différentes applications qui communiquent les unes avec les autres. À ces niveaux, on parle d'applications et de processus.

**PDU** : Données

### Couches 4 - Transport

Cette couche fournie des protocoles qui sont consommés par les applications afin de déterminer les modalités selons lesquelles l'information est échangé entre les systèmes. Cette couche est gérée par le noyau du système d'exploitation.

**Port** : Introduit à ce niveau, le port est un identifiant numérique à 16 bits utilisé par le noyau du système d'exploitation afin d'identifier le processus vers lequel un message est destiné. 

**PDU** : Segment pour *TCP*, Datagramme pour *UDP*

### Couches 3 - Réseau

Cette couche fournie les mécanisme et les outils permettant à l'information de traverser les différents réseaux afin de se rendre au système de destination.

**Adresse IP** : Introduite à ce niveau, l'adresse IP permet d'identifier un système. Cette adresse est utilisée par les routeurs afin de déterminer le chemin qu'un paquet doit prendre pour se rendre à destination.

**Routeur** : Le routeur opère à ce niveau.

**PDU** : Paquet

### Couches 2 - Liaison

Cette couche fournie les mécanisme et les outils nécessaire pour effectuer une communication point à point. La couche permet à un système de communiquer avec un ou plusieurs systèmes dans son environnement immédiat. Les informations définies ici ne sont pas suffisantes pour traverser différents réseaux. 

**Adresse MAC** : Adresse physique d'une interface connectée au réseau. Permet d'identifier un participant immédiat dans une communication.

**Commutateur** : Le commutateur utilise les adresses MAC afin d'acheminer les trames Ethernet aux bons systèmes. Le commutateur n'utilise pas de règles logiques pour déterminer où envoyer les trames - il ne fait que surveiller l'activité réseau et mémoriser qui se trouve où.

**PDU** : Trame Ethernet

### Couches 1 - Physique

Cette couche définie les standards physiques permettant la communication : les câbles utilisées, quel fil dans un câble est utilisé à quelle fin, les vitesses de transmissions, le format des signaux électriques, etc...

**PDU** : Bits

## Encapsulation

Les protocoles d'un niveau spécifique vont communiquer avec les niveaux équivalents sur l'hôte de destination

<img src="img/Pasted image 20260130133959.png" width="700" />

Lorsqu'un hôte communique avec un autre, la pile va être utilisée dans sont entièreté.

Les messages que l'on envoie sont d'abord générés par l'application qui procède à la communication réseau (niveau 7). Le message va ensuite descendre le long de la pile, se faisant ajouter des informations à chaque niveau. 

Les informations ajoutés sont utilisés par les équipement intermédiaire et finaux afin de transmettre le message jusqu'au système de destination, puis ensuite au processus de destination. 

<img src="img/Pasted image 20260114105322.png" width="700" />
Le système de destination va recevoir les bits composant le message au niveau de la couche physique. Les données vont monter tout le long de la pile. À chaque niveau monté, un PDU en est extrait. 
### Analogie boiteuse

**Situation** : _Alice_, employée chez **Initech**, doit transmettre un rapport TPS à _Bob_, employé chez **OrgTech**.

1. **Production du contenu (couches 7–6–5 : Application / Présentation / Session)**  
    Alice rédige le rapport TPS selon les règles d’affaires d’Initech : format du document, langue, structure, et contenu.  
    **Le contenu est prêt à être transmis, mais il n’a encore aucune information de livraison.**

2. **Identification du destinataire final (couche 4 : Transport)**  
    Alice place le rapport dans une enveloppe indiquant clairement :  _Bob – Bureau 404_.  
    Cette enveloppe permet à OrgTech de savoir **à quelle personne précise** le document doit être remis une fois arrivé.  
    **Comme un numéro de port, elle identifie le bon “service” chez le destinataire.**

3. **Adressage vers l’organisation (couche 3 : Réseau)**  
    L’enveloppe est ensuite placée dans une boîte de livraison portant l’adresse complète de l’entreprise :  _OrgTech, 1234 rue Org, X1X 1X1_.  
    La réception d’Initech remet cette boîte à la compagnie de transport.  
    **L’adresse permet d’atteindre le bon édifice, mais pas encore la bonne personne.**

4. **Acheminement local par le transporteur (couches 3 et 2 : Réseau / Liaison)**  
    La compagnie de transport lit l’adresse et le code postal pour choisir l’itinéraire général (couche 3).  
    À chaque centre de tri, la boîte reçoit une étiquette interne (numéro de camion, quai, etc), utilisée uniquement pour le transport local (couche 2).  
    Un livreur transporte alors la boîte vers l’entrepôt suivant.
   
5. **Relais successifs (couches 2 et 3)**  
    Cette étape se répète plusieurs fois : chaque centre de tri enlève l’étiquette locale précédente et en appose une nouvelle pour le prochain trajet, jusqu’à atteindre l’adresse finale.

6. **Arrivée chez OrgTech – fin de l’acheminement réseau**  
    La réception d’OrgTech reçoit la boîte.  
    Puisque la boîte a rempli son rôle (amener le courrier à la bonne adresse), elle est ouverte et mise de côté.  
    **L’information d’adressage global n’est plus nécessaire.**
   
7. **Distribution interne (couche 4 : Transport)**  
    La réception lit l’enveloppe interne et constate qu’elle est adressée à _Bob, Bureau 404_.  
    Elle l’achemine donc vers le bon bureau, comme le système d'exploitation l'acheminerait au bon service via le bon port.

8. **Exploitation du contenu (couches 5–6–7)**  
    Bob ouvre l’enveloppe, lit le rapport et l’utilise selon les règles d’affaires d’OrgTech.  


<hr>

[Suivant](cours02-02-transmission-reseau.md)