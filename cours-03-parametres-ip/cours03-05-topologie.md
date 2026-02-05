**Situation** : Le système "Source" envoie un message au système "Destination".

<img src="img/Pasted image 20260130134759.png" width="700" />

1. Le système `Source` génère un message aux niveaux applicatifs (*Niveaux 7-6-5*)
2. Le message est encapsulé dans un segment TCP (*Niveau 4*)
3. Le segment est encapsulé dans un paquet IP. L'adresse de destination du paquet est l'adresse ip du système `Destination` - le 10.0.0.2. (*Niveau 3*)
	1. L'adresse IP du paquet va être l'adresse IP de la destination finale.
4. En se servant de son masque de sous-réseau, le système `Source` détermine si le système `Destination` est dans le même réseau.
5. Comme `Destination` n'est pas dans le même réseau, la communication doit passer par un intermédiaire - la passerelle par défaut de `Source`.
	1. Dans notre situation, la passerelle est le routeur `R1`.
	2. Lorsque vous entendez passerelle, pensez routeur.
6. Le paquet IP va être encapsulé dans un trame Ethernet. La trame va contenir l'adresse physique (MAC) de `R1`. (*Niveau 2*)
7. Le message est maintenant prêt à être envoyé. Le commutateur va regarder la trame Ethernet et utilisé l'adresse MAC pour relayer le message à `R1`.
8. `R1` va désencapsuler le message jusqu'à l'obtention du paquet IP. Il utiliser l'adresse IP afin de déterminer le chemin à emprunter.