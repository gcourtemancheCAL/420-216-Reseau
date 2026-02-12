# Le paquet IP

IP fournit l'adressage logique et les mécanismes permettant l'acheminement entre réseaux. Les paquets IP sont encapsulés dans des trames de niveau 2 (ex.: Ethernet).

## L'en-tête IP

L'en-tete IPv4 contient les informations nécessaires au routage, a l'identification et au contrôle d'erreur.

<img src="img/Pasted image 20260209113933.png" width="800" />

### TTL

Le TTL (Time To Live) limite la durée de vie d'un paquet. Chaque routeur le décremente; lorsqu'il atteint 0, le paquet est supprimé. Un message ICMP peut etre renvoyé pour indiquer l'erreur.

Les valeurs par défaut vont généralement être l'une de 64, 128 ou 255.

### Protocol

Ce champ indique le protocole de couche supérieure encapsulé (ex.: TCP = 6, UDP = 17, ICMP = 1). Il permet au destinataire de bien interpréter le type de données reçues et de les acheminer au bon service.

### Checksum

Le checksum vérifie uniquement l'en-tête IP. Il est recalculé à chaque routeur puisque le TTL change.

Le checksum IP est calcué en en additionnement ensemble chaque groupe de 16 bits de l'en-tête ip. 

La retenu est ensuite ajouté au résultat finale.

[Exemple](https://en.wikipedia.org/wiki/Internet_checksum#Examples)

Le checksum est retiré du paquet IPv6 - l'exercice étant jugé redondant et peu utile. 

### Les adresses

L'en-tête contient l'adresse IP source et l'adresse IP destination. Ces adresses servent au routage de bout en bout, contrairement aux adresses MAC qui sont locales au lien.

L'adresse IP source permet au récipiendaire d'identifier l'hôte vers lequel envoyé la réponse.

## Fragmentation et MTU

La MTU (Maximum Transmission Unit) est la taille maximale de charge utile que le lien peut transporter. Si un paquet IP est plus grand que la MTU et que la fragmentation est permise, il est fragmenté en plusieurs paquets. Le réassemblage se fait uniquement à la destination.

## Modes d'adressage

Comme pour Ethernet, IP supporte plusieurs modes d'adressage selon l'adresse de destination.

### Unicast

Paquet envoyé a une seule adresse IP. C'est le mode le plus courant.

### Diffusion (broadcast)

Paquet envoye a l'adresse de broadcast d'un reseau (ex.: 192.168.1.255). Les routeurs ne relaient pas les broadcasts par défaut.

L'adresse de broadcast dans un réseau est celle où tous les bits de la partie hôte sont à `1`

e.g : 
- Pour le réseau `192.168.0.0/24`, l'adresse de diffusion est le `192.168.0.255`.
- Pour le réseau `192.168.128.0/18`, l'adresse de diffusion est le `192.168.191.255`.

### Multicast

Paquet envoye à une adresse multicast (224.0.0.0/4). Les hôtes qui se sont abonnés au groupe reçoivent le paquet.

Les adresses multicast dans le 239.0.0.0/8 sont privées à une organisation.

