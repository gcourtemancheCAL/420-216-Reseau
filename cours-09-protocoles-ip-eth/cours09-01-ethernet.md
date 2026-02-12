# Ethernet

## La trame Ethernet - IEEE 802.3

Une trame Ethernet transporte les donnees du niveau superieur (ex.: IP). Elle est composee d'un en-tete, de la charge utile et d'un champ de controle d'erreur.

<img src="img/Pasted image 20260209111200.png" width="800" />

La trame Ehernet peut contenir jusqu'à approximativement 1500 octets de données.

### Le préambule

Le préambule est une sequence de bits alternés (101010...) qui sert a synchroniser l'horloge de l'emetteur et du recepteur avant la lecture de la trame.

La séquence utilisée est le `10101010` pour chacun des octets menant au SFD.

#### SFD

Le SFD (Start Frame Delimiter) indique la fin du preambule et le debut effectif de la trame. Il aide le recepteur a aligner correctement les bits qui suivent.

La séquence utilisée par le SFD est le `10101011`. 

### Les adresses src et dst

La trame contient l'adresse MAC de destination et l'adresse MAC source. Le commutateur (switch) utilise ces adresses pour faire l'acheminement local.

Le commutateur va utiliser l'adresse MAC source afin d'identifier le port auquel est connectée une adresse MAC.

### Le FCS

Le FCS (Frame Check Sequence) est un champ de controle d'erreur base sur un CRC. Il permet de detecter la plupart des erreurs de transmission.

Le CRC est calculé par l'emetteur sur le contenu de la trame. Le recepteur recalcule le CRC et compare les valeurs.

L'algorithme de CRC se base sur la division modulo-2 via l'application succésive de XOR. Le principe est extrêmement simple à implémenter au niveau hardware et peut être fait *inline* sans réellement ajouté de latence.

 [Visualisation en ligne](https://www.youtube.com/watch?v=iwj8ZgyzqZk)

[Exemple](https://en.wikipedia.org/wiki/Cyclic_redundancy_check#Computation)

#### Qu'est-ce qui arrive si une erreur est détectée?

Si le CRC ne correspond pas, la trame est rejetée et n'est pas remontée aux couches supérieures. Il n'y a pas de retransmission au niveau 2 (Ethernet).

## Modes d'adressage

Selon l'adresse de destination, une trame peut viser un hôte précis, tous les hôtes, ou un groupe d'hôtes.

### Unicast

Trame envoyée a une seule adresse MAC. Le switch ne la transmet que vers le port associé a cette adresse.

### Diffusion (broadcast)

Trame envoyée a l'adresse MAC de diffusion (FF:FF:FF:FF:FF:FF). Elle est livrée à tous les hotes du domaine de diffusion.

### Multicast

Trame envoyée a une adresse MAC de multicast. 

Dépendemment de la switch et de leur configuration, la trame peut être retransmise à tous les hôtes y étant connectés ou bien seulement à ceux qui sont abonnées au groupe de mulicast. Dans tous les cas, seuls les hôtes qui se sont abonnés au groupe acceptent la trame.

## Domaine de diffusion

Le domaine de diffusion correspond a l'ensemble des hôtes qui recoivent les trames de broadcast. On va typiquement parler de tous les hôtes connectés à un même commutateur.

Les routeurs séparent les domaines de diffusion.