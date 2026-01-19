## Configuration IP statique
Configuration IP dans laquelle les paramètres sont définis statiquement sur l'hôte.

**Avantages** :
- Adresse fixe
- Adresse connue

**Inconvénients** :
- On doit s’assurer manuellement qu’elle soit unique sur le réseau
- L’adresse IP utilisée doit respecter les normes du LAN
- Peut causer problèmes en cas de changement de réseau (portables, appareils mobiles)
## Configuration IP dynamique
Configuration IP dans laquelle les paramètres sont assignés à un hôte automatiquement par le biais d'un serveur dédié à cette tâche.

On appel ce type de serveur un serveur DHCP.

**Avantages** :
- Simple et automatique
- Les normes du réseau sont toujours appliquées

**Inconvénients** :
- Adresses variables / manque de stabilité

### Réservation d'adresse

Il est souvent possible - sur un serveur DHCP - de réserver des adresses à des hôtes spécifiques. Lorsque possible, cela vient palier aux principal inconvénient de l'allocation dynamique d'adresse.

<img src="img/Pasted image 20260115161755.png" width="600" />

### DHCP 

- Protocole d’attribution dynamique des adresses IP
- Au moment d’initialiser sa connectivité IP, un client va envoyer une requête d'adresse IP en mode broadcast (à tout le monde sur le réseau).
- Le serveur DHCP va alors lui offrir une adresse à partir d’un « pool » d’adresses dynamiques.
	- Des adresses IP réservées à une attribution dynamique

**Paramètres configurables via DHCP** : 
- Adresse IP
- Masque réseau
- Passerelle par défaut
- Serveur DNS

#### Bail DHCP

Afin de faciliter la réutilisation des adresses, les configurations DHCP sont assignés pour un certains lapse de temps seulement.

La période durant laquelle une configuration DHCP est réservée à un hôte est appelée un "bail".

À l'expiration de son bail, si l'hôte est encore connecté au réseau, il peut demander à le renouveller.

##### Commandes de manipulation du bail DHCP

Sur windows : 
- `ipconfig /release` : Libère la configuration IP pour toutes les interfaces
- `ipconfig /release "adapter"` : Libère la configuration IP pour toutes les interfaces correspondant au texte passé en argument. 
- `ipconfig /renew` : Renouvelle la configuration IP de toutes les interfaces
- `ipconfig /renew "adapter"` : Renouvelle la configuration IP de toutes les interfaces correspondant au texte passé en argument. 

N.b. On peut spécifier le nom complet d'une interface ou un pattern avec wildcards. Exemples : 
- `ipconfig /release "Ethernet"` : Libère la configuration IP de l'interface Ethernet.
- `ipconfig /release "*Ethernet*"` : Libère la configuration IP de toutes les interfaces ayant le texte Ethernet dans leur nom.

`ipconfig /all` : Affiche, entre autres, les informations de bail DHCP.

<img src="img/Pasted image 20260119085807.png" width="600" />

#### Adresse APIPA

En cas d'échec du processus DHCP, Windows va s'auto-assigner une adresse IP entre 169.254.0.1 et 169.254.255.254 (169.254.0.0/16). 

On appel cette adresse auto-assignée une adresse APIPA (_Automatic Private IP Addressing_) . 

#### Processus DHCP

**Étape 1** Le client demande diffuse une requête de configuration IP

<img src="img/Pasted image 20260115161119.png" width="800" />

**Étape 2** Le serveur DHCP propose une configuration IP

<img src="img/Pasted image 20260115161323.png" width="800" />


**Étape 3** Le client confirme les paramètres au serveur

<img src="img/Pasted image 20260115161411.png" width="800" />

**Étape 4** Le serveur confirme l'assignation des paramètres

<img src="img/Pasted image 20260115161503.png" width="800" />

<hr>

[Précédent](cours03-04-param-ip.md) - [Suivant](cours03-06-configuration-ip-windows.md)





