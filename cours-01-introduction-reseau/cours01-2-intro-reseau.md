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

#### Topologie en arbre

<img src="img/Pasted image 20260114095411.png" width="600" />

**Définition** : Topologie hiérarchique qui se compose de plusieurs topologies en étoiles reliées entres elles.

### Schématisation d'un réseau

#### Topologie d'un réseau résidentiel typique
<img src="img/Pasted image 20260113153926.png" width="600" />

#### Topologie plus complexe

<img src="img/Pasted image 20260113155323.png" width="800" />

#### Vocabulaire

**Noeud** : Un appareil dans notre topologie qui peut créer ou transmettre de l'information. Peut être un appareil terminal (pc, tablette, cellulaire, serveur) ou bien un appareil intermédiaire (commutateur, routeur, point d'accès sans-fil, modem, concentrateur )

**Lien/connexion** :  Médium de communication reliant deux noeuds ensemble et leur permettant de communiquer entre eux. Il doit y avoir un lien entre deux noeuds pour qu'ils puissent communiquer entre eux. Le lien peut représenter une connexion câblée physique (câble ethernet, câble coaxial, fibre optique, connexion série, ...) ou bien, aussi, une connexion sans-fil (WiFi, bluetooth, zigbee, ...)

<hr>

[Précédent](cours01-1-presentation) - [Suivant](cours01-3-types-de-reseaux)