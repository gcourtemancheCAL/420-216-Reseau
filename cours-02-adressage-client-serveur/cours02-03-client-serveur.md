# Architecture Client/Serveur

## Architecture client/serveur

Dans cette architecture, un "serveur" va être disponible sur le réseau. Le serveur va attendre que des "clients" s'y connecte afin de leur offrir un service quelquonque.

**Le serveur** : Le service est disponible et attend qu'un client initie la communication.

**Le client** : Le client va envoyer une demande de service à un serveur.

Le serveur ne va pas nécessairement connaitre les clients qui vont faire affaire avec lui. Les clients peuvent s'ajouter et se retirer comme bon leur semble. 

<img src="img/Pasted image 20260114145832.png" width="400" />

'Client' et 'serveur' sont des rôles qui sont occupés dans le contexte d'une communication. Ces rôles peuvent être utilisés pour décrire autant un logiciel qu'un système en soit.

**Exemple** : 

Alice utilise le navigateur "chrome" pour envoyer une requête http afin d'obtenir une page web hébergé par www.example.com via un serveur nginx.

On peut dire que : 
	Le programme "chrome" est un _client_ http.
	Le service (un programme) nginx est un serveur http.

On peut aussi dire que :
	Alice est le client.
	www.example.com est le serveur.

Dépendemment  du contexte, un même système peut jouer le rôle de client ou de serveur

<img src="img/Pasted image 20260114150800.png" width="800" />

Dans cet exemple, le système "server" va faire office de serveur lorsque les différents client lui envoient des requêtes. Dans son intéraction avec la base de données, il va être le client et la base de donnée va être le serveur.

## Autres architectures
### Architecture Master/Slave

Architecture commune dans les contextes embarqués dans laquelle un appareil (le maitre) est responsable de gérer les autres appareils (esclaves) en leur envoyant des requêtes et des directives.

Dans cette architecture, le maitre doit connaitre tous les systèmes esclaves. Les systèmes esclaves attendent des instructions du maitre.


<img src="img/Pasted image 20260114143145.png" width="700" />

Ce genre d'architecture se présente beaucoup lorsqu'il est question de systèmes résilients et distribués (serveurs web, base de données, etc...).

### Architecture peer to peer

Architecture sans autorité centralisée, les différents participant se 'découvre' et s'organise mutuellement.

<img src="img/Pasted image 20260114143431.png" width="700" />

**Exemple** : bittorrent, freenet


<hr>

[Précédent](cours02-02-transmission-reseau.md) - [Suivant](cours02-04-services-communs.md)