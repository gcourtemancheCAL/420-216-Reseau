# Configuration de pare-feu pour le cours de Réseau

## Contexte

Windows a progressivement renforcé sa sécurité au fil des années. De nos jours, les installations modernes de Windows vont donc avoir une polique de pare-feu assez stricte afin de protéger le système moyen des attaques. Ces politiques sont bonnes et réfléchis dans le contexte d'une utilisation normale d'un ordinateur Windows par une personne moyenne. 

Dans notre cas à nous, certaines politiques qui était présente par défaut auparavant sont maintenant désactivé, ce qui va venir nous empêcher de réaliser certaines manipulations durant nos laboratoires. Nous allons donc devoir créer une exemption - très limité en portée - au sein de notre pare-feu.

## Ajout de la règle de pare-feu

En utilisant la boite de dialogue d'exécution de programme (Win + R), lancez le programme `wf.msc`

<img src="img/Pasted image 20260204100126.png" width="600" />

Dans le menu `Règle entrante`, choissez "Nouvelle règle"

<img src="img/Pasted image 20260204100434.png" width="600" />

Choisissez une règle sur mesure : 

<img src="img/Pasted image 20260204100514.png" width="600" />

Dans protocoles et ports, choisissez `ICMPv4` comme type de protocole

<img src="img/Pasted image 20260204100556.png" width="600" />

Dans profile, cochez uniquement `Privée`

<img src="img/Pasted image 20260204100621.png" width="600" />

Vous pouvez appeler la règle comme vous le voulez. Appuyez sur `Terminer`

<img src="img/Pasted image 20260204100719.png" width="600" />

## Portée de la règle

La règle va autoriser certaines entrées réseaux mais uniquement sur les réseaux "Privées". Ces réseaux vont généralement être limités à vos résidentiel.

Pour les activité en classe, il va falloir s'assurer que le réseau auquel vous êtes connectés soit identifié comme un réseau privée pour que cette règle prenne effet. Voir instructions dans "cours04-03 -configuration-ip-windows".