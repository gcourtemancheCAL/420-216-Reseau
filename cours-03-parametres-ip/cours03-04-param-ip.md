# Paramètres IP d'un hôte

## Définitions

**Hôte** : Appareil connecté au réseau pouvant envoyé et recevoir des données. Un hôte est identifié par une adresse IP.

**Adresse IP** : Adresse identifiant un hôte sur le réseau. Une adresse IP est routable - c'est à dire qu'à partir de cette information, on peut traverser différents réseaux afin de se rendre à destination. Analogue à une adresse résidentielle.

**Passerelle par défaut** : Adresse IP du routeur qui est utilisé par un hôte. La passerelle par défaut va avoir comme responsabilité d'acheminer les messages en destination d'un autre réseau.

**Serveur DNS** : Serveur auquel le système va envoyer ses requêtes DNS.

**Masque réseau** : Masque binaire permettant d'isoler la partie d'une adresse IP identifiant le réseau auquel il fait partie. Le masque réseau est utilisé par un hôte lorsqu'il veut envoyer un paquet afin de déterminer si le destinataire est dans le même réseau que lui. 

Si le destinataire est dans le même réseau, le paquet peut lui être directement acheminé. Autrement, le paquet devra passer par la passerelle par défaut.

**N.B. L'adresse réseau est un concept local à un système. Un hôte ne va pas connaitre l'adresse réseau utilisée par les autres hôtes avec lesquels il communique.**

**Pour voir les paramètres IP des interfaces d'un système :**
- **Sur Windows** : `ipconfig`
- **Sur Linux** : `ip address`

## IPv4 et IPv6

Deux versions du protocole IP sont présentement utilisés en parallèle sur nos réseaux : IPv4 et IPv6. IPv6 est le successeur à IPv4. Pour des raisons historiques, bien qu'existant depuis longtemps, IPv6 n'est pas encore déployé et supporté partout.

**Nous allons exclusivement travailler avec IPv4 dans ce cours.** 

Les concepts que nous allons voir vont - pour la plupart - s'appliquer à IPv6.

La principale différence que vous devez apprendre : le format des adresses.

Une adresse IPv4 est d'une longueur de 32 bits que l'on représente sous la forme de 4 octets écrit en décimal séparés d'un point.

**e.g. 192.168.0.1**

Une adresse IPv6 est d'une longueur de 128 bits que l'on représente sous la forme de 8 groupes de 4 octets écrit en hexadécimal séparés par des ":". 

**e.g. 2001:0db8:0000:0000:0000:ff00:0042:8329**

La représentation d'une adresse IPv6 peut être compressée 
- En omettant les 0 au début de chaque groupe de 4 octets
- En utilisant une la chaine `::` afin de représenter une longue séquence de 0. 

**e.g. 2001:db8::ff00:42:8329**

## Cas spécial : Le 0

Le `0.0.0.0` sur IPv4, et le `00:00:00:00:00:00:00:00` (abbrévié `::`) ne sont pas des adresses assignables. 

Cependant, elles sont souvent utilisés lors de la configuration de différents services dans le but de représenter n'importe quelle adresse.

## Cas spécial : Adresse de loopback

Il existe une catégorie d'adresse IP que l'on appel **adresses de loopback**.

Ces adresses sont analogues aux interfaces de loopback - elles permettent à un système de communiquer avec lui-même.

<img src="img/Pasted image 20260115113647.png" width="500" />

En IPv4, toutes les adresses du réseau `127.0.0.0/8` sont des adresses de loopback. Typiquement, on va utiliser le `127.0.0.1`

Sur IPv6, on utilise le `00:00:00:00:00:00:00:01`, souvent abbrévié `::1`


<hr>

[Précédent](cours03-03-supports-physiques.md) - [Suivant](cours03-05-topologie.md)
