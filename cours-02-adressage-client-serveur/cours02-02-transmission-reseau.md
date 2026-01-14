# Exemple de transmission avec une topologie simple
## La topologie

<img src="img/Pasted image 20260114132432.png" width="700" />

## La situation

_Client_ veut envoyer un message à _ServeurWeb_

<img src="img/Pasted image 20260114133619.png" width="700" />


1. Client va générer les données de son message (Couches 7-6-5)
2. Client va insérer les données de son message dans un datagramme UDP (couche 4). Ce datagramme va contenir le numéro de port de destination (le service sur le serveur web) et le numéro de port de réponse (le logiciel sur le client)
3. Le datagramme va être inséré dans un paquet IP (couche 3). Ce paquet va contenir l'adresse IP du ServeurWeb.  Cette adresse va être utilisée pour acheminé le paquet jusqu'à destination.
4. Le client va insérer le paquet IP dans une trame Ethernet (couche 2). Cette trame Ethernet va être adressée à l'adresse MAC du routeurA. 
	1. Pourquoi le routeur A? 
		1. Ethernet peut juste acheminer les messages dans le même réseau.
		2. C'est le routeur qui est responsable d'acheminer les messages vers d'autres réseaux
5. Les bits composant la trame Ethernet sont transmises via un signal digital sur le câble ethernet reliant Client à Commutateur1 (couche 1).

<img src="img/Pasted image 20260114134603.png" width="700" />

6. Commutateur1 reçoit la séquence de bit de Client. (couche1)
7. Il décode ces bits en trame Ethernet. (couche 2)
8. Il sait sur quel port l'adresse MAC de destination est connecté. Il redirige le flux de bit vers ce port (couches 2 et 1).

<img src="img/Pasted image 20260114134811.png" width="700" />

9. RouteurA reçoit la séquence de bit de Commutateur1. (couche1)
10. Il décode ces bits en trame Ethernet. Ensuite, il en extrait le paquet IP(couche 2 et 3)
11. Il utilise l'adresse IP de destination afin de déterminer le chemin vers le serveur Web. (couche 3)
12. Il insère le paquet IP dans une nouvelle trame Ethernet, celle-ci adressée vers RouteurB. (couche 2)
13. Les bits composant la trame Ethernet sont transmises via un signal digital sur le câble ethernet reliant RouteurA à RouteurB (couche 1)

<img src="img/Pasted image 20260114135322.png" width="700" />

9. RouteurB reçoit la séquence de bit de RouteurA. (couche1)
10. Il décode ces bits en trame Ethernet. Ensuite, il en extrait le paquet IP(couche 2 et 3)
11. Il utilise l'adresse IP de destination afin de déterminer le chemin vers le serveur Web. (couche 3). 
12. Comme RouteurB et ServeurWeb sont dans le même réseau, RouteurB sait qu'il peut envoyer le message directement à ServeurWeb.
13. Il insère le paquet IP dans une nouvelle trame Ethernet, celle-ci adressée vers ServeurWeb. (couche 2)
14. Les bits composant la trame Ethernet sont transmises via un signal digital sur le câble Ethernet reliant RouteurB à Commutateur2 (couche 1)


<img src="img/Pasted image 20260114135512.png" width="700" />

15. Commutateur2 reçoit la séquence de bit de RouteurB. (couche1)
16. Il décode ces bits en trame Ethernet. (couche 2)
17. Il sait sur quel port l'adresse MAC de destination est connectée. Il redirige le flux de bit vers ce port (couches 2 et 1).

<img src="img/Pasted image 20260114135717.png" width="700" />

18. ServeurWeb reçoit la séquence de bit de Commutateur2. (couche1)
19. Il en décode la trame Ethernet. (couche2)
20. De cette trame, il en extrait le paquet IP. (couche3)
21. De ce paquet IP, il en extrait le datagramme UDP (couche4).
22. À l'aide du numéro de port de destination, il sait vers quel processus acheminer les données. (couche4)
23. Le processus reçoit les données et les gère. (couche 5-6-7).

<hr>

[Précédent](cours02-01-pile-osi.md) - [Suivant](cours02-03-client-serveur.md)