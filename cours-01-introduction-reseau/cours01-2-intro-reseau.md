# Introduction aux réseaux
## Le réseau
**Définition** : un réseau est un ensemble de système interconnecté afin de permettre l'échange et le partage de resources et de services.

**Exemples de resources :** 
- Fichiers
- Base de données
- Imprimantes
- Courriels
- Serveurs de jeux
## Topologie d'un réseau
**Définition** : l'organisation des équipements dans un réseau. Peut être utilisé pour parler de la schématisation d'un réseau. 
### Types de topologies
#### Topologie point à point
<img src="img/Pasted image 20260113145823.png" width="300" />
**Définition** : Topologie simple dans laquelle deux systèmes sont directement connectés l'un à l'autre.

#### Topologie en bus

<img src="img/Pasted image 20260113150300.png" width="300" />

**Définition** : Topologie simple dans tous les systèmes envoient les messages sur un bus commun. Tout le monde peut voir ce qui passe sur le bus et est responsable de seulement gérer ce qui le concerne.

De nos jours : surtout utilisé dans les contextes hardware.

#### Topologie maillé (en maille)
<img src="img/Pasted image 20260113151021.png" width="300" />

**Définition** : Topologie dans laquelle tous les systèmes sont directement connectés à tous les autres systèmes.

Difficile d'ajouter des systèmes dans les contextes où la connexion est filaire. Aujourd'hui, surtout restreint à certains types de réseau sans-fil (mesh networking)

#### Topologie en étoile
<img src="img/Pasted image 20260113153104.png" width="300" />

**Définition** : Topologie dans laquelle les systèmes sont connectés à un appareil central qui est responsable d'acheminer les communications à leurs destinataires.

### Schématisation d'un réseau

#### Topologie d'un réseau résidentiel typique
<img src="img/Pasted image 20260113153926.png" width="600" />
#### Topologie plus complexe
![[Pasted image 20260113155323.png]]
#### Vocabulaire
**Noeud** : Un appareil dans notre topologie qui peut créer ou transmettre de l'information. Peut être un appareil terminal (pc, tablette, cellulaire, serveur) ou bien un appareil intermédiaire (commutateur, routeur, point d'accès sans-fil, modem, concentrateur )

**Lien/connexion** :  Médium de communication reliant deux noeuds ensemble et leur permettant de communiquer entre eux. Il doit y avoir un lien entre deux noeuds pour qu'ils puissent communiquer entre eux. Le lien peut représenter une connexion câblée physique (câble ethernet, câble coaxial, fibre optique, connexion série, ...) ou bien, aussi, une connexion sans-fil (WiFi, bluetooth, zigbee, ...)

## Types de réseaux
#### PAN
**Personnal area network**
Réseau de très courte portée. E.g. Bluetooth.

#### LAN
**Local area network**
Réseau restreint à un domicile, un bâtiment, ou une entreprise. Le LAN est restreint dans la surface géographique qu'il occupe. Les appareils vont être connectés entre eux par le biais d'équippement comme des commutateurs ou des routeurs.

#### CAN
**Campus area network**
Un réseau contrôlé par une même organisation composé de plusieurs LAN occupant des bâtiments différents dans un espace géographique relativement restreint.

Réfère généralement au type de réseau que l'on peut voir dans certaines universités qui ont un campus distribué à travers la ville ou dans certaines entreprises qui peuvent avoir des bureaux dans plusieurs bâtiments différents.
#### WAN
**Wide area network**
Réseau occupant un large espace géographique.

L'internet est souvent donné en exemple du principal WAN.

## Équipement d'un réseau
### Câbles et connecteurs

#### Câble ethernet avec connecteur RJ45
<img src="img/Pasted image 20260113161030.png" width="300" />

#### Câble coaxial
<img src="img/Pasted image 20260113161210.png" width="300" />


#### Fibre optique
<img src="img/Pasted image 20260113161313.png" width="300" />

#### Câble série
<img src="img/Pasted image 20260113161410.png" width="300" />


### Équipements intermédiaires

#### Concentrateur
<img src="img/Pasted image 20260113164128.png" width="300" />

**Obsolète**
Équipement permettant de concentrer la communication de plusieurs appareils différents sur une même ligne.

#### Commutateur
<img src="img/Pasted image 20260113164435.png" width="300" />

Permet d'établir une connexion directe entre différents appareils. Contrairement au concentrateur, la ligne n'est pas partagée.

<img src="img/Pasted image 20260113153104.png" width="300" />

#### Routeur

<img src="img/Pasted image 20260113164956.png" width="600" />

Achemine les messages à travers les différents sous-réseaux ou réseaux. Un routeur va servir de passerelle vers internet.

<img src="img/Pasted image 20260113172223.png" width="600" />
