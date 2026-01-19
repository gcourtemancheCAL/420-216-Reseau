# Atelier : Configuration IP sur Windows

#### Préalables : 
- Ces exercices sont à réaliser en classe, en équipe.
- Deux membres de l'équipe devront chacun se connecter à un routeur.
- Les configurations vont être mises en place sur machine virtuelle Windows. Si vous n'avez pas de machines virtuelles de prêtes, effectuez les configurations directement sur votre système. **Vous allez, cependant, être responsable de l'état de votre système après coup.**

#### Préparation de la machine virtuelle

Afin que les exercices ci-dessous puissent fonctionner, vous devez vous assurer que la machine virtuelle est dans le même réseau IP que votre ordinateur. Assurez-vous que son adaptateur réseau soit en mode "pont" avec votre adaptateur Ethernet.

<img src="img/Pasted image 20260119093431.png" width="600" />

#### Étape 1 - Connexion des machines au réseau: 

**Objectifs de l'étape** : 
- Connecter les équipements physiques au routeur
- Valider la connexion IP
- Valider la connectivité entre les systèmes

**Procédure à suivre** : 
- Connectez-vous, par câble, au routeur mis à votre disposition.
- **Validations** : 
	- En regardant les indicateurs lumineux, validez que la connexion est bien établie.
	- Validez les paramètres IP qui vous ont été assignés par DHCP. Votre adresse devrait être entre le `192.168.100.10` et le `192.168.100.20`.
- Changez le profile de réseau pour "Réseau privé"

<img src="img/Pasted image 20260119093755.png" width="600" />

- **Validation** :
	- Une fois cette étape réalisée sur les deux ordinateurs physiques, ils devraient être en mesure de se rejoindre avec la commande ping.

````bash
# Utilisation : ping <adresse de destination>.
# E.g. : 
 ping 192.168.0.1 # Envoie des paquets ICMP vers le 192.168.0.1
 ping www.google.com # Envoie des paquets ICMP vers www.google.com

#'ping' est une commande permettant de valider la connectivité entre deux 
# systèmes. Lorsque vous utilisé la commande 'ping', votre système va envoyer des 
# messages ICMP vers sa destination. Sur réception, le système de destination va 
# répondre avec ses propres paquets ICMP.

# Ce processus permet de valider que le chemin d'aller-retour entre deux systèmes 
# est fonctionnel.

# L'utilitaire 'ping' va afficher le résultat de chaque requête icmp. Les cas de 
# figures suivants sont possibles :
# - Une réponse a été obtenue de l'hôte de destination.
# - Une réponse indiquant une erreur a été obtenue d'un équipement intermédiaire.
# - Aucune réponse n'a été obtenue.

````

#### Étape 2 - Connexion des machines virtuelles au réseau: 

**Objectifs de l'étape** : 
- Connecter les machines virtuellles au réseau.

**Procédure à suivre** : 
- Démarrer les machines virtuelles.
- Répéter la même procédure qu'à l'étape 1 mais cette fois-ci sur les machines virtuelles.
	- **QUESTION** : Est-ce que vous allez avoir besoin de connecter la machine virtuelle par câble? Pourquoi? D'où vient sa connexion au réseau?

**Validation** : 
- À la fin de cette étape, chacune des machines physiques devraient pouvoir rejoindre chacun des machines virtuelles.
- Les machines virtuelles devraient pouvoir se rejoindre entre elles.

#### Étape 3 - Manipulation du DHCP : 

**Cette étape est à réaliser sur l'une de vos machines virtuelles.**

**Objectifs de l'étape** : 
- Pratiquer les commandes permettant d'intéragir avec le DHCP

**Procédure à suivre** : 
- À l'aide des commandes vuent en classe, identifiez les paramètres IP obtenus par DHCP.
	- L'adresse IP
	- L'adresse réseau
	- La passerelle par défaut
	- Le serveur DNS
- En utilisant les commandes vuent en classe, identifiez les informations suivantes : 
	- Date d'émission du bail DHCP
	- Date d'expiration du bail DHCP
	- Durée du bail DHCP
- En utilisant les commandes vuent en classe, libérez les paramètres IP obtenus par DHCP.
- À l'aide des commandes vuent en classe, identifiez la configuration IP de votre interface.
	- **QUESTION** : Qu'est-ce que vous remarquez?
- À l'aide des commandes vuent en classe, demandez une nouvelle configuration IP via DHCP. Validez ensuite les paramètres qui vous ont été alloués.
	- **QUESTION** : Est-ce qu'il y a des différences au niveau des paramètres IP? Au niveau du bail? Expliquez pourquoi.

**Validation** : 
- À la fin de cette étape, chacune des machines physiques devraient pouvoir rejoindre chacun des machines virtuelles.
- Les machines virtuelles devraient pouvoir se rejoindre entre elles.

#### Étape 4 - Configuration statique : 

**Cette étape est à réaliser sur l'une de vos machines virtuelles.**

**Objectifs de l'étape** : 
- Pratiquer les manipulations nécessaires à la réalisation d'une configuration IP statique.

**Procédure à suivre** : 
- Configurez l'une de vos machines virtuelles avec la configuration IP statique suivante  :
	- Adresse IP : 192.168.100.50
	- Masque réseau : 255.255.255.0
	- Passerelle par défaut : 192.168.100.1
	- DNS : 192.168.100.1
- À l'aide des commandes vuent en classe, validez la nouvelle configuration IP.
- Utilisez les commandes vuent en classe pour relâcher l'adresse IP et en demander une autre. 
	- **QUESTION** : Que se passe-t-il? Pourquoi?

**Validation** : 
- À la fin de cette étape, chacune des machines physiques devraient pouvoir rejoindre chacun des machines virtuelles.
- Les machines virtuelles devraient pouvoir se rejoindre entre elles.
#### Étape 5 - Conflit d'adresse : 

**Objectifs de l'étape** : 
- Visualiser une erreur de configuration réseau.
- Mieux comprendre les inconvénients d'une allocation statique d'adresse.

**Procédure à suivre** : 
- Configurez vos deux machines virtuelles avec la configuration IP statique suivante  :
	- Adresse IP : 192.168.100.50
	- Masque réseau : 255.255.255.0
	- Passerelle par défaut : 192.168.100.1
	- DNS : 192.168.100.1
- À l'aide des commandes vuent en classe, validez la nouvelle configuration IP.
- Sur vos deux machines virtuelles, en même temps, testez la connectivité avec le routeur (le 192.168.100.1). Répétez ce test à quelques reprises.
	- **QUESTION** : Que se passe-t-il? Formulez une hypothèse expliquant ce que vous voyez.