# Services communs
## Serveurs web

### http

Protocole de base pour le web. Le client envoie une requête http identifiant le contenu désiré. Le serveur répond avec une réponse http contenant le résultat de l'opération.

**Protocole de transport** : TCP
**Port par défaut** : 80

[Cliquez ici pour plus de détails sur http](https://developer.mozilla.org/fr/docs/Web/HTTP/Guides/Overview)

#### Exemple de requête http : 

<img src="img/Pasted image 20260114151715.png" width="500" />


**La méthode** : Identifie le type de requête effectué.
**Le chemin** : Identifie la resource sur laquelle on opère.

Dans le contexte le plus simple, le chemin identifie le chemin d'un fichier que l'on veut recevoir.

**Méthodes valides : **

1. **GET**
2. *HEAD*
3. *OPTIONS*
4. *TRACE*
5. PUT
6. DELETE
7. **POST**
8. PATCH
9. *CONNECT*

**GET** et **POST** sont les méthodes les plus communes. **PUT** et **DELETE** sont assez communes. **PATCH** est une méthode un peu plus récente (ajouté au standard en 2010)

[Cliquez ici pour plus d'informations sur les méthodes http](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Methods)

#### Exemple de réponse http : 

<img src="img/Pasted image 20260114153510.png" width="500" />
_NB : Le contenu de la réponse suivrait l'en-tête ci-dessus._

Le **code de status** et le **message de status** indiquent le résultat de l'opération. Un même code de status est toujours accompagné du même message.

Les codes de status sont classifiés en 5 catégories : 

1. 1xx -> Information
2. 2xx -> Succès
3. 3xx -> Redirection
4. 4xx -> Erreur client
5. 5xx -> Erreur serveur

[Cliquez ici pour plus d'information sur les codes de status.](https://developer.mozilla.org/fr/docs/Web/HTTP/Reference/Status)

#### Les clients http : 
- Les navigateurs (ou fureteurs) web
	- chrome, opera, firefox, edge, ...
- Certains outils de ligne de commande
	- curl, wget
- Des clients http peuvent être intégrés à plusieurs applications différentes
	- Applications mobiles, steam, ...
- D'autres serveurs web
	- Il est train commun qu'un serveur web consomme un api http fourni par un autre serveur http.
	- Dans ce cas, le premier serveur web va être client d'un second serveur web.

#### Les serveurs http : 
- apache, nginx, tomcat, ...

### https

Variante sécurisée de http. La communication https est chiffrée via les mécanismes fournies par TLS.

**Protocole de transport** : TCP
**Port par défaut** : 443

## Base de données

Les bases de données vont être disponible par le biais du réseau à l'aide d'un protocole qui est propre à chaque fournisseur.

### Oracle

**Nom du protocole** : SQL\*Net
**Protocole de transport** : TCP
**Port** : 1521

### Postgresql

**Nom du protocole** : PGWire
**Protocole de transport** : TCP
**Port** : 5432

### MongoDB

**Nom du protocole** : MongoDB Wire Protocol
**Protocole de transport** : TCP
**Port** : 27017

## Serveur mail

Plusieurs protocoles différents sont utilisés pour faire fonctionner l'échange de courriel en ligne.

### SMTP

Permet l'envoie de courriel. Les différents clients mails vont utiliser SMTP pour envoyer un courriel vers un serveur. Les serveurs mails vont utilisés pour s'échanger les courriels entres-eux.

**Transport** : TCP
**Ports utilisés** : 25 (entre serveurs), 587 (non chiffré), 465 (chiffré).

**Exemple de serveur SMTP** :
- postfix
- msmtp
- sendmail
### IMAP

Permet à un client d'aller chercher les courriels qui lui sont destinés sur un serveur mail. 

**Transport** : TCP
**Ports utilisés** : 143 (non chiffré), 993 (chiffré).

**Exemple de serveur IMAP ** :
- Dovecot
- Courrier

### POP3

Permet à un client d'aller chercher les courriels qui lui sont destinés sur un serveur mail. À la différence de IMAP, les courriels sont supprimés du serveur une fois récupérés.

**Transport** : TCP
**Ports utilisés** : 110 (non chiffré), 995 (chiffré).

**Exemple de serveur POP3 ** :
- Dovecot
- Courrier
### Client mail 
- Outlook
- Thunderbird 

## DNS

Un serveur DNS va retourner l'adresse IP assignée à un nom de domaine particulier.

**Nom de domaine** : Identifiant hiéarchique textuel identifiant un système via sa correspondance à une adresse IP. 

Exemples : 
1. www.google.com
2. www.youtube.com

**Exemple de serveur DNS** :
- bind9

Google offre des serveurs DNS publique au 8.8.8.8 et au 8.8.4.4

**Transport** : TCP et UDP
**Ports utilisés** : 53.

<hr>

[Précédent](cours02-03-client-serveur.md)