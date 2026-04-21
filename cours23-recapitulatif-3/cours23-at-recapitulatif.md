# Cours 23 - Atelier récapitulatif #3

Répliquez la topologie suivante et effectuez les configurations demandées.

### Matériel requis
- 1 laptop Windows
- 1 ordinateur debian
- 1 commutateur nortel
- 1 routeur cisco
- 6 câble Ethernet
- Câbles série

<img src="img/Pasted%20image%2020260415135024.png" width="800" />

### Avant de commencer
Avant de commencer la configuration des équipements, effacer leur configuration.
- Retournez au "factory-default" sur le commutateur Nortel.
- Exécutez la commande "write erase" sur le routeur.
- Exécutez le script "reset.sh" sur le système linux.

### Tests
Testez l'ensemble de votre réseau au fur et à mesure. Pensez à tester le plus tôt et le plus fréquemment possible. Le plus vite vous détectez un problème, le plus facile il sera à régler.

### Précisions sur la topologie
- Vous allez devoir gérer les équipements dans "Subnet Personnel" et "Serveur Personnel"
- Le LAN de la classe est déjà préconfiguré. Vous devrez vous y connecter. 
	- Votre routeur devra se connecter au commutateur SW1 à l'arrière. 
	- Votre serveur devra se connecter au commutateur SW2 à l'arrière.
	- Utilisez les prises murales noires et le panneau de brassage
- Utilisez les commutateurs Nortel.

#### Configurations à réaliser

##### Subnet Personnel

Le réseau "Subnet Personnel" doit être un sous-réseau du 192.168.48.0/20. Ce sous-réseau devrait être le plus petit sous-réseau possible pouvant accueillir 180 hôtes. 

##### Routeur Subnet 

- Son interface connectée au LAN de la classe est à configurer via DHCP
	- C'est de là que va provenir sa route par défaut
	- C'est aussi cette interface qui sera l'interface "outside"
- Configurez un NAT.
- Configurez un serveur DHCP 
	- Utilisez les 8.8.8.8 comme serveur DNS
- Configurez les routes nécessaires au bon fonctionnement du réseau
	- Votre laptop va devoir pouvoir rejoindre votre serveur.
	- Votre laptop va devoir avoir accès à internet.

##### Windows

- Configurez votre système Windows à l'aide de DHCP

##### Serveur

- Assignez une configuration statique à votre serveur. 
	- Il devrait avoir le 10.0.1.X/23 (où X = # de prise murale) comme adresse.
	- Sa passerelle devrait être le 10.0.0.1.
	- Utilisez le 1.1.1.1 comme serveur DNS.
- Installez les services suivants : 
	- openssh-server
	- apache2
- Déployez le site web fourni dans le dossier "res" sur votre serveur Web. Utilisez sftp afin de la transférer à partir de votre laptop Windows.

