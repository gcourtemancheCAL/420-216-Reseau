# Exercice 1 - Intro à Packet Tracer

Cet exercice à comme objectif de s'introduire à l'utilisation de packet tracer. Complétez le fichier `ex1-intro.pkt` en suivant les consignes.

## Configuration de PC-B

Le PC B n'est présentement pas configuré. Nous allons vouloir lui assigner une adresse IP et une passerelle par défaut.

### Identifier l'adresse de la passerelle

1. Ouvrez le CLI du routeur.
2. Afficher le status des interfaces avec la commande `show ip interface brief`
3. Observez le résultat. Le routeur dispose de 2 interfaces GigabitEthernet. L'une est `down` et l'autre est `up`. En regardant la topologie, identifiez laquelle de ces deux interfaces est connectée au PC B.
4. L'adresse IP de cette interface va-t-être l'adresse de la passerelle du PC B.

### Configurer le PC

Ouvrez l'onglet `desktop` du PC B. Configurez ses paramètres IP à l'aide de l'outil `Ip Configuration`.

Son adresse IP est le 192.168.1.2 avec un masque sur 24 bits.

Sa passerelle a été identifié à l'étape précédente.

Il n'y a pas de serveur DNS.

### Validation

Vous devriez pouvoir `ping` la passerelle avec le PC en utilisant l'outil `Command Prompt`

## Connexion du routeur au commutateur

Nous allons vouloir connecter le routeur au commutateur. Cependant, il n'y a qu'une seule interface sur le commutateur d'activée. Il va donc falloir identifier laquelle est la bonne.

En utilisant l'outil `cli` sur le commutateur, affichez le status des interfaces à l'aide de la commande `show ip interface brief`.

Certaines interfaces sont `administratevely down`. Cela signifie qu'elles ont été désactivées de force au niveau de la configuration.

Vous pouvez le valider à l'aide des commandes suivantes : 

```
enable
show running-config
```

La commande `enable` va vous faire entrer en mode privilégié. La commande `show running-config` va vous permettre de visualiser la configuration actuelle. La plupart des interfaces devraient avoir la configuration `shutdown`, ce qui signifie qu'elles sont désactivées.

Connectez maintenant le routeur - son interface GigabitEthernet 0/0 à l'interface active du commutateur. 


### Validation

Le voyant lumineux de la connexion devrait devenir vert après quelques moments.

## Connexion du PC au commutateur

### Activation d'une interface

Nous allons activer une interface sur le commutateur afin d'y connecter le PC A.

Sur l'interface CLI du commutateur, entrez en mode privilégié si vous ne l'êtes pas déjà.

Vous pouvez le déterminer par la présence du `#` à la droite de l'invite de commande.

Ensuite, nous allons entrer en mode de configuration : `configure terminal`.

Observez le changement de l'invite de commande une fois rendu en mode configuration.

Nous allons maintenant entrer en mode *configuration d'interface* afin d'activer l'interface `fastEthernet 0/1` en entrant la commande suivante : `interface fastEthernet 0/1`

Une fois en mode *configuration d'interface*, nous allons utiliser la commande `no shutdown` afin d'activer l'interface.

La commande `no` permet de retirer des configurations. Ainsi, nous retirons la configuration `shutdown` qui désactive une interface. 

### Sauvegarde de la configuration

Utilisez la commande `exit` jusqu'à ce que vous soyez de retour au mode privilégié.

Dans ce mode, utilisez la commande `copy running-config startup-config` afin de sauvegarder la configuration.

Le `running-config` est la configuration en cours d'exécution. Les changements de configuration se font sur cette config.

Le `startup-config` est un fichier contenant la configuration à charger au démarrage de l'équipement.

### Connexion et configuration du PC.

Connectez le PC A au commutateur sur son interface FastEthernet 0/1.

Configurez les paramètres IP du PC A. Vous devriez être en mesure de trouver les paramètres à entrer par vous même. L'adresse de la passerelle va être l'adresse de l'interface du routeur qui est connecté au commutateur (i.e. dans le même réseau).

### Validation

PC A devrait pouvoir ping PC B.


## Ajout d'un PC au commutateur

Ajoutez un PC et connectez le au commutateur. Vous allez devoir : 
- Faire une configuration statique sur le PC. Choisissez une adresse IP dans le bon réseau.
- Le connecter au commutateur
- Activez l'interface en question sur le commutateur.
- Sauvegarder la configuration du commutateur.

## Changement de configuration sur le routeur

Sur le routeur, effectuez les changements de configurations suivants : 
- Changer le nom d'hôte pour le "ex1Routeur".
- Changer la bannière de *message of the day* pour le message suivant : Ceci est un message du jour.

Utilisez l'aide du routeur (la touche ?) pour voir les commandes disponibles ainsi que les arguments utilisés. 

Pour valider la bannière, entrez la commande exit jusqu'au moment de voir `press RETURN to get started` puis appuyer sur enter.