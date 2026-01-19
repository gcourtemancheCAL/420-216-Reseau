# Adaptateurs réseau, interfaces, et liens
## Adaptateur réseau

**Définition** : Composant matériel permettant la connexion au réseau. La connexion peut être filaire (câble ethernet, fibre optique, coaxial, ...) ou sans-fil.

Un système peut disposer de plusieurs adapteurs réseaux de même type ou de type distinct.

Un adaptateur peut être intégré directement à la carte mère :

<img src="img/Pasted image 20260115090523.png" width="500" />

Un adaptateur peut aussi être sur une carte d'extension dédiée : 

<img src="img/Pasted image 20260115091142.png" width="500" />

Ou même une clé USB : 

<img src="img/Pasted image 20260115091354.png" width="500" />

## Port/Lien

Un port est l'emplacement physique dans lequel un câble est branché pour établir une connexion.

**Port ethernet** : 

<img src="img/Pasted image 20260115091816.png" width="500" />

Un lien est le terme générique pour un point de connexion au réseau. 

**Exemples** : 
1. Un port Ethernet permet d'établir un lien Ethernet.
2. Un adaptateur réseau WiFi permet d'établir un lien WiFi.

### Adresse MAC

Adresse unique à un lien réseau. L'adresse MAC est "brulée" dans l'adaptateur réseau. Pour un adaptateur réseau multiport, chaque port dispose de sa propre adresse MAC.

On la connait aussi sous le nom **d'adresse physique**.

Une adresse MAC est composée de 6 octets. On les affiche typiquement sous une forme hexadécimal où chaque octet est séparé du suivant par un trait d'union.

<img src="img/Pasted image 20260115094316.png" width="500" />

Les 3 premiers octets identifient le manufacturier. Les 3 derniers octets sont une séquences uniques déterminer au moment où la carte est fabriquée.

<img src="img/Pasted image 20260115094809.png" width="500" />

#### Identifier le manufacturier de votre adresse adapteur

[Exercice - Identifier le manufacturier de votre adapteur](cours03-01-adapteurs-intf.md)

## Interface

**Définition** : Représentation logique d'un point de connexion au réseau. De façon générale, une interface va représenter un port physique ou, dans le cas d'un interface sans-fil, un adaptateur. 

Une interface peut être virtuelle (aucun mapping physique réel). Plusieurs interfaces peuvent être multiplexé sur le même port.

## Interface de loopback

Une interface virtuelle spéciale peut exister qui pointe toujours vers votre propre machine. On appelle cette interface l'interface de loopback.

<img src="img/Pasted image 20260115113647.png" width="500" />

## Identification des interfaces
### Windows

La commande `ipconfig` va vous présenter les informations d'interfaces. Les noms utilisés sont, cependant, très peu descriptifs.

<img src="img/Pasted image 20260115111818.png" width="500" />

La commande `ipconfig /all` va donner plus de détails - incluant une description de l'interface.

<img src="img/Pasted image 20260115112253.png" width="700" />

On peut voir ici que l'interface Ethernet 3 est une interface virtuelle utilisée par Virtual Box.

### Linux

La commande `ip link` va afficher les informations sur les liens. 

<img src="img/Pasted image 20260115105249.png" width="900" />

#### Nomenclature des interfaces

La nomenclature des interfaces suit un model précis sous linux. 

L'interface de loopback est appelée **lo** et peut potentiellement être suivie d'un numéro de séquence.

Les interfaces détectées automatiquement par le système d'exploitation vont se faire donner un nom basé sur le type d'adaptateur et le type de connexion au système.

**Préfixe selon le type d'interface** : 
- Ethernet : en 
- Wireless : wl 
- Wide Area Wireless (satellite) : ww

**Suffixe selon le mode de connexion** : 
- Intégré à la carte mère : o#
- PCI Express hotplug : s#
- Carte d'extension PCI : p#s#

**Le `#` est remplacé par un numéro de séquence.**

**Exemples de cas typiques** : 
- Un adaptateur Ethernet intégré à la carte mère : eno1
- Une carte d'extension Ethernet PCIe  : enp1s1
- Un adaptateur sans-fil intégré à la carte mère : wlo1

Les noms d'interfaces "eth#" et "wlan#" étaient anciènnement le standard et peuvent encore apparaitrent dans certains contextes.

D'autres préfixes et suffixes peuvent exister dans des situations plus niches ou spécifiques.

<hr>

[Suivant](cours03-02-donnees-et-transfert.md)