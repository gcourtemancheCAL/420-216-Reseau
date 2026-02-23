
## Perte de message

Les paquets IP peuvent se perdre en route (aucune garantie de livraison)

**Causes possibles**
- Corruption du signal (layer 1)
	- Interférence sur les câbles Ethernet
	- Interférence sur le signal WiFi
	- Peut être détecté au niveau 1 ou 2
- Équipement intermédiaire surchargé
	- L'équipement intermédiaire peut "drop" des paquets lorsque surchargé
- Équipement défectueux

<img src="img/Pasted image 20260211090049.png" width="800" />

## Désordre

L'ordre de livraison n'est pas garanti. Il est possible que 2 paquets empruntent un chemin différent (la route emprunté n'est pas garanti non plus!).

Lorsqu'un paquet est fragmenté, le offset de fragment permet la reconstruction du paquet en ordonnant les fragments. Les problèmes vis à vis l'ordre de réception surviennent lorsqu'on envoit plusieurs paquets distincts.

<img src="img/Pasted image 20260211090059.png" width="800" />

## Duplication

Il est possible qu'un message soit dupliqué. 

Cela peut être causé par la retransmission d'un message qui tardait à arriver.

Il est aussi possible qu'un équipement intermédiaire achemine un message par plusieurs interfaces en même temps.

