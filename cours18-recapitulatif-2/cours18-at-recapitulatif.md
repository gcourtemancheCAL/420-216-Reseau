# Cours 18 - Atelier récapitulatif #2

Répliquez la topologie suivante et effectuez les configurations demandées.

### Matériel requis
- 1 laptop Windows
- 1 ordinateur debian
- 1 carte réseau supplémentaire
- 1 commutateur nortel
- 1 routeur cisco
- 5 câble Ethernet
<img src="img/Pasted image 20260316130730.png" width="800" />
### Avant de commencer
Avant de commencer la configuration des équipements, effacer leur configuration.
- Retournez au "factory-default" sur le commutateur Nortel.
- Exécutez le script "reset.sh" sur le système linux.

### Tests
Testez l'ensemble de votre réseau au fur et à mesure. Pensez à tester le plus tôt et le plus fréquemment possible. Le plus vite vous détectez un problème, le plus facile il sera à régler.

### Précisions sur la topologie
- Le réseau de la classe est déjà préconfiguré. Vous y avez accès via l'une des prises murales noires. Vous devrez compléter les connexions avec le panneau de brassage.
- Utilisez les routeurs dans le rack à l'arrière.
- Utilisez les commutateurs Nortel.
- Le X dans vos adresse IP est le numéro de la prise murale à laquelle vous aller connecter votre routeur.
- Toutes vos configurations vont être statiques.
	- Les adresses IP de vos hôtes vous sont données.
	- Utilisez le 1.1.1.1 comme serveur DNS.
	- Vous allez devoir trouver les masques réseaux et l'adresse de la passerelle à partir des informations qui vous sont données dans le schéma.
- Vous devrez ajouter une deuxième interface à votre serveur Linux. Les deux interfaces vont devoir être configurées.
- Votre routeur va devoir avoir une route par défaut.

### Services à installer :
Installez les services suivant sur le serveur linux : 
- openssh-server
	- Désactivez les connexions root
	- Changez le port et l'adresse d'écoute pour le 192.168.X.101:60022
- vsftpd
	- Activez l'écriture des fichiers
	- Changez l'adresse d'écoute pour le 192.168.X.102
- apache2
	- Changez l'adresse d'écoute pour les deux valeurs suivantes : 
		- 127.0.0.1:80
		- 192.168.X.101:80
- avahi-daemon
	- Configurez le nom d'hôte mDNS suivants :
		- web-VOTRENOM.local
			- Devrait pointer vers le 192.168.X.101

### Page web :

À partir de votre système Windows :
- Dans le dossier site-web, se trouve une page à transférer sur le système Linux.
- Transférez la sur le système Linux à l'aide de sftp.
- Effectuez les manipulations nécessaires afin que je puisse y accéder en utilisant l'url `http://web-VOTRENOM.local/cours18`

