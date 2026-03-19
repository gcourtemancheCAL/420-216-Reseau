# PacketTracer


## Téléchargement 

https://www.netacad.com/resources/lab-downloads?courseLang=en-US

## Sujets : 

- L'interface de packet tracer
- Ajouter des équipements
	- PC, serveur, routeur, commutateur
	- Ajouter des interfaces à de l'équipement
- Connectés les équipements
- Les modes simulation et temps réel
- Configurer les équipements
	- Le menu `Desktop`
	- Le menu `CLI`

## Le CLI de Cisco IOS

**Note très importante** : en tout temps vous pouvez appuyer sur la touche `?` pour avoir de l'information supplémentaire. Cela peut vous montrer les commandes disponibles dans le mode actuel, ou vous montrer les arguments possibles pour la commande que vous êtes en train d'écrire.

Le CLI de Cisco IOS est modal : 
- À travers les commandes exécutés, vous allez naviguez certains modes d'exécutions.
- Chaque mode a des commandes qui lui sont spécifiques
- Certaines commandes peuvent vous permettre de naviguer d'un mode à l'autre.

On peut imaginer que l'on traverse un menu "invisible." À tout moment on peut appuyer sur la touche `?` pour voir les options de notre menu.

Différents types d'équipements vont avoir différentes fonctionalités et donc différentes commandes vont pouvoir être utilisées. 

<img src="img/Pasted image 20260311135021.png" width="800" />

### Quelques commandes utiles

**De façon générale :**
- Pour interrompre une commande : ctrl + shift + 6

**Mode non-privilégié (et privilégié) :**
- Afficher le détail des interfaces : 
	- `show interfaces`
	- `show ip interface`
- Afficher le résumé des interfaces : `show ip interface brief`
- Rejoindre un hôte : `ping hostname`
	- Nécéssite une interface ip de configuré.

**Mode privilégié :**
- Afficher la configuration actuelle :`show running-config`
- Afficher la configuration sauvegardé :`show startup-config`
- Sauvegarder la configuration : `copy running-config startup-config`
	- Peut être abbrévié : `copy run start
