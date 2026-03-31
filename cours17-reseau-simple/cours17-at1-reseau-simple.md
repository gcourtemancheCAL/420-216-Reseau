# Exercice pratique - Prise en main d'un commutateur Nortel en console série

## Objectif

Cet atelier a comme objectifs de voir les pratiques suivantes : 
- L'utilisation d'une connexion en série afin de configurer de l'équipement de réseau
- Configurer un commutateur nortel
	- Activer désactiver ses interfaces
	- Sauvegarder la configuration
- Effectuer une configuration simple sur un routeur Cisco
	- Identifier les interfaces
	- Configurer une interface statiquement
	- Configurer une route par défaut
	- Sauvegarder sa configuration
- Pratiquer la configuration d'un système Linux

---

## Matériel requis

Par équipe :

- 1 commutateur Nortel
- 1 routeur Cisco
- 1 ordinateur Windows avec PuTTY installé;
- 1 système Debian
- 1 câble console 
- 1 adaptateur USB-série
- 5 câbles Ethernet;

---

## Branchements initiaux

- Branchez le commutateur à l'alimentation électrique. Il devrait démarrer de lui-même.
- Ajoutez une carte Ethernet supplémentaire à votre système Linux.
- Branchez vos équipements (par Ethernet) de sorte à reproduire la topologie suivante : 

<img src="img/Pasted image 20260312092709.png" width="400" />

## Configuration IP des équipements

Nous allons devoir commencer par identifier le réseau IP dans lequel nous allons travailler.

**Voici la marche à suivre pour cet exercice :**
- Plus tard, nous allons connecter un routeur à l'une des prises murale noire. Identifiez le # de la prise que vous allez utiliser.
- Votre réseau IP sera le `10.0.X.0/24`, où X est le numéro de la prise à laquelle vous allez connecter votre routeur.

Assignez des configurations IP statiques à vos deux systèmes en gardant en tête le consignes suivantes  :
- Vous devez configurer *toutes* les interfaces connectées au commutateur.
- Chaque interface doit avoir une adresse IP différente
- Normalement, la première adresse d'un réseau est réservée au routeur.
- Assignez la passerelle immédiatement, même si elle n'existe pas encore.
- Utilisez le `1.1.1.1` comme serveur DNS.

## Configuration initiale du commutateur

Nous ne savons pas dans quel état se trouve le commutateur. Nous allons donc recommencer à neuf en en effaçant sa configuration.

### Connexion série

1. Utilisez le câble USB-Série afin de connecter votre poste Windows au commutateur.
2. Dans le gestionnaire des périphériques, identifiez le nom du port COM correspondant.
3. En utilisant PuTTY, connectez-vous en série par le biais du port COM identifié.
4. Une fois connecté au commutateur, appuyez sur CTRL+Y pour démarrer la session.

### Effacer la configuration

Une fois la connexion au commutateur établie, entrez les commandes suivantes afin de supprimer la configuration et redémarrez la commutateur : 

```
enable
restore factory-default
```

### Validation

Une fois le commutateur démarré, vous devriez être en mesure de `ping` les deux interfaces de votre système Linux à partir de votre système Windows.


## Configuration d'interfaces

À l'aide de PuTTY, nous allons jouer un peu avec la configuration des interfaces sur le commutateur. Nous allons principalement désactiver et activer des interfaces afin de voir les résultats de ces manipulations.

### Désactiver l'ensemble des interfaces

Nous allons commencer en désactivant toutes les interfaces du commutateur. Entrez les commandes suivantes sur votre commutateur : 

```
	enable
	configure terminal
	interface fastEthernet 1-24
	shutdown
```

**Ces commandes vont (en ordre) :**
1. Activer le mode privilégié
2. Entrer en mode de configuration
3. Entrer en mode configuration d'interface pour les interfaces fastEthernet 1 à 24.
4. Désactiver ces interfaces

À la suite de ces commandes, nous allons être encore en mode de configuration d'interfaces.

**Observations :**
- Regardez les indicateurs lumineux sur le commutateur. Comment se comportent-ils?
- Regarder le status des interfaces sur votre système Linux. Qu'est-ce que vous voyez?

**Nous allons maintenant afficher le status des interfaces.**

**À l'aide de PuTTY :**
- Quittez les modes de configurations d'interface et de configuration afin de retourner au mode privilégié.
- Dans ce mode, afficher le status des interfaces :  `show interfaces`
- Normalement, toutes les interfaces devraient être désactivées.

#### Validation

Vous ne devriez plus être en mesure de `ping` les deux interfaces de votre système Linux à partir de votre système Windows.

### Activer les interfaces utilisées

**Nous allons maintenant réactiver les interfaces auxquelles nos équipements sont connectées.**

**À l'aide de PuTTY :**
- Pour chacune des interfaces que vous voulez activer, entrez en mode de configuration d'interfaces.
	- Vous pouvez le faire une à la fois, ou bien plusieurs en même temps.
- Dans ce mode, réactivez les interfaces à l'aide de la commande `no shutdown`.
- Retournez en mode privilégié afin de valider le résultat.

**Observations :**
- Regardez les indicateurs lumineux sur le commutateur. Comment se comportent-ils?
- Regarder le status des interfaces sur votre système Linux. Qu'est-ce que vous voyez?

#### Validation

Vous devriez être en mesure de `ping` les deux interfaces de votre système Linux à partir de votre système Windows.

## Configuration initiale du routeur Cisco

1. Branchez le routeur à l'alimentation et allumez-le.
2. Connectez votre système Windows au port console du routeur Cisco à l'aide du câble USB-Série et de l'adaptateur Série-RJ45.
3. Établissez une connexion série à l'aide de PuTTY.
4. Appuyez sur la touche `Enter` afin de débutter la session.

Nous allons commencer par effacer la configuration du routeur Cisco. Entrez les commandes suivantes : 

```
enable
write erase
reload
```

Le routeur devrait redémarrer. Pendant qu'il redémarre, branchez son interface 10BaseT à l'un des ports de votre commutateur.

La topologie de votre réseau devrait ressembler à celle-ci : 

<img src="img/Pasted image 20260312110456.png" width="400" />

Activez ce port sur votre commutateur et sauvegardez la configuration du commutateur à l'aide de la commande  `write memory`

**Observations :**
- Portez attention, sur votre commutateur, au voyant lumineux du port auquel est connecté le routeur. Que remarquez-vous?

## Configuration initiale du routeur Cisco

### Nom des interfaces

Le routeur que vous utilisez dispose de 2 interfaces Ethernet : 
- L'une 10Base-T -> Ethernet
- L'autre 100Base-T -> FastEthernet

Différents modèles vont avoir différents types d'interface. Il est important de bien les identifier selon les libellés sur les ports.

### Configuration de l'interface connectée au commutateur

La configuration initiale d'un routeur désactive toute ses interfaces. Nous allons donc devoir configurer les interfaces du routeur en lui assignant sa configuration IP et en l'activant.

À l'aide d'une connexion série via PuTTY, connectez-vous au routeur. 

Identifiez les interfaces disponible sur votre routeur à l'aide de la commande `show ip interface brief`

Nous allons commencer par configurer l'interface qui est connectée au commutateur. Comme nous avons connecté l'interface 10Base-T au commutateur, nous recherchons l'interface avec le nom "Ethernet"

Entrez les commandes suivantes, en prenant soin de bien identifier l'interface à configurer : 

```
enable
configure terminal
interface <nom de l'interface>
ip address <adresse du routeur> 255.255.255.0
no shutdown
```

Nous allons utiliser la première adresse de votre réseau comme adresse du routeur. Cela devrait donner quelque chose comme 10.0.X.1

**Ces commandes vont (en ordre) :**
1. Activer le mode privilégié
2. Entrer en mode de configuration
3. Entrer en mode configuration d'interface pour l'interface Ethernet. 
4. Assigner une adresse IP et un masque réseau à cette interface. Toutes les interfaces d'un routeur que l'on utilise doivent avoir une adresse IP valide dans un réseau distinct.
5. Activer l'interface.

Retournez au mode privilégié et sauvegardez votre configuration avec la commande `copy running-config startup-config`

Affichez le status des interfaces à l'aide de la commande `show ip interface brief`. Validez le résultat.

Affichez le status de la configuration sauvegardé à l'aide de la commande `show startup-config`. Votre interface Ethernet devrait avoir la configuration que vous venez de lui assigner.

**Observations :**
- Regardez l'indicateur lumineux du port auquel est connecté le routeur. Que remarquez-vous maintenant?

#### Validation

Les systèmes Windows et Linux devraient être en mesure de rejoindre le routeur avec `ping`.

## Connexion du routeur au reste du réseau

1. Connectez l'interface FastEthernet de votre routeur à la prise murale précédemment identifiée.
2. Configurez cette interface avec l'adresse suivante : `10.0.0.X/24`, où le X est le numéro de la prise murale.
3. Activez l'interface
4. Sauvegardez la configuration.

### Validation

À partir du routeur, vous devriez être en mesure de `ping` le système se situant au `10.0.0.101`.

## Création de la route par défaut

Votre routeur va être en mesure de faire le pont entre les réseaux de ses différentes interfaces - aucune configuration particulière n'est requise. 

Vous pouvez d'ailleurs le valider avec vos systèmes Windows et Linux en essayant de rejoindre le `10.0.0.101`.

Cependant, pour aller "plus loin", il va falloir configurer des routes sur votre routeur pour lui indiquer quel chemin prendre pour rejoindre certains réseaux.

Aujourd'hui, nous allons uniquement voir la route par défaut. La route par défaut d'un routeur est la route empruntée automatiqument si aucune autre route ne convient. C'est l'équivalent de la passerelle par défaut de votre routeur.

En mode configuration, sur votre routeur, entrez la commande suivante : 
```
ip route 0.0.0.0 0.0.0.0 10.0.0.101
```

Nous allons voir plus tard le détail de l'anatomie d'une route. Pour le moment, il suffit de comprendre que cette route définiée le `10.0.0.1` comme "passerelle" à notre routeur. Ainsi, tout trafic passant par notre routeur à destination autre que le `10.0.X.0/24` et `10.0.0.0/24` va être réacheminé vers le `10.0.0.101`.

Une fois cette route ajoutée, sauvegardez votre configuration.

### Validation

À partir de vos système Windows et Linux, vous devriez avoir accès à internet.