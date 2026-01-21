# Nom d'hôte et de domaine

## Qu'est-ce qu'un nom d'hôte?

**Définition :** Identifiant lisible par l'humain donné à un système sur un réseau. Il sert à distinguer les appareils entre eux (ex. `serveur-web`, `poste-01`, `nas-maison`). Un nom d'hôte est local au domaine ou au réseau qui le définit.

**Format :**
	- chaînes alphanumériques, séparées par des tirets (`-`), sans espaces. 
	- Insensibles à la casse 

## Qu'est-ce qu'un nom de domaine?

**Définition :** Espace de nommage hiérarchique utilisé pour nommer et retrouver des ressources sur Internet ou un réseau privé. 
	**Exemples :**
		- `www.reddit.com`
		- `claurendeau.qc.ca`

**TLD (Top Level Domain) :** Suffixe de plus haut niveau (ex. `.com`, `.org`, `.ca`, `.edu`). Gérés par des registres et l'ICANN/autorités nationales.

**Sous-domaine :** Division logique d'un domaine pour organiser des services ou des sites (ex. `app.example.com`, `eu.wiki.org`). Chaque point introduit un niveau hiérarchique supplémentaire.

**Quelques exemples :**
- `www.google.com` → hôte `www`, domaine `google.com`, TLD `.com`
- `maps.google.com` → sous-domaine `maps`
- `www.claurendeau.qc.ca` → sous-domaine `www`, domaine `claurendeau.qc.ca`, TLD `.ca`
- `intra.societe.local` → domaine privé `.local`

## Les serveurs DNS

### Comment ça fonctionne

DNS (Domain Name System) traduit les noms de domaine en adresses IP

Le processsur de résolution est récursif : un résolveur interroge d'abord les serveurs racine, puis les serveurs TLD, puis les serveurs faisant autorité pour le domaine ciblé, jusqu'à obtenir l'enregistrement demandé.

Les réponses sont mises en cache. C'est à dire qu'elles sont conservées pour réduire la latence et le trafic pour les requêtes subséquentes.

L'architecture DNS est de type client/serveur : lorsqu'un système veut effectuer une résolution DNS, il va envoyer une requête vers un serveur DNS.

Le serveur DNS qui dans votre configuration IP est le serveur DNS utilisé par défaut par votre système.
### Types de records

Les enregistrement sont organisés sous forme de record de type distinct. Quelques exemples de type de record : 
- **A** : nom → IPv4
- **AAAA** : nom → IPv6
- **CNAME** : alias vers un autre nom canonique
- **NS** : serveurs faisant autorité pour le domaine
- **SOA** : informations de zone (numéro de série, contacts)
- **PTR** : résolution inverse (IP → nom)
- ...

### Reverse DNS lookup

Associe une adresse IP à un nom via un enregistrement PTR.

## Le fichier "hosts"

#### Qu'est-ce que c'est?

Le fichier "hosts" est un fichier local qui  qui map statiquement des noms vers des adresses IP avant de consulter DNS.

**Emplacement :**
	**Sur Windows :** `C:/Windows/System32/drivers/etc/HOSTS`
	**Sur Linux :** `/etc/hosts`

#### Syntaxe d'une entrée
```
<adresse IP> <nom> [alias1 alias2 ...]
# Exemple
192.168.1.10 intranet.local intranet
127.0.0.1    localhost
```
## mDNS

mDNS est un mécanisme permettant à des nom de domaine d'être défini sur un réseau local sans demander de configuration ou la présence d'un serveur DNS privé.

Un système peut rouler un service mDNS qui va diffuser en broadcast sur le réseau un enregistrement DNS spécifique. Ainsi, les hôtes sur le réseau local vont pouvoir résoudre ce nom de domaine sans nécéssiter de configuration particulière de leur côté.

mDNS est spécifique aux réseaux locaux. mDNS utilise le suffixe (tld) `.local`.

mDNS est très utile pour partager des resources dans un petit réseau local tel qu'un réseau résidentiel ou une petite entreprise.

<img src="img/Pasted image 20260120145752.png" width="800" />

## Processus de résolution de domaine sous Windows

### Les étapes suivies par Windows
1. Cache DNS local
2. Consulte le fichier `hosts` local.
3. Si le tld est `.local`, Windows va diffuser une requête mDNS sur le réseau local.
4. Interroge les serveurs DNS configurés sur l'interface active.

#### La cache DNS

Windows dispose d'une cache DNS. Les résultats de requête DNS sont donc sauvegarder afin d'accélérer les opérations subséquentes. 

**Pour vider la cache DNS :** `ipconfig /flushdns`

### Les étapes suivies par Linux

Les étapes suivis par Linux sont en réalité variables et configurables.

La définition de la marche à suivre se situe dans le fichier `/etc/nsswitch.conf`.  Typiquement, la résolution du nom de domaine va se produire dans l'ordre suivant : 
1. Consultation du fichier `/etc/hosts`
2. Consulation du serveur DNS configuré

Notez bien que, par défaut, Linux ne vas pas disposer de cache DNS. 

De plus, vous devez avoir un client mDNS d'installer pour pouvoir utiliser la résolution mDNS. Le cas échéant, l'ordre sera typiquement le suivant : 
1. Fichier `/etc/hosts`
2. mDNS
3. Serveur DNS

#### /etc/resolv.conf

Les configurations de serveur DNS sont globales au système Linux (et non pas par interface). Elles se situent dans le fichier `/etc/resolv.conf`

<img src="img/Pasted image 20260120150947.png" width="800" />

## nslookup

### Description
Outil en ligne de commande pour interroger un serveur DNS. `nslookup` permet la résolution directe (domaine vers ip) ou inverse (ip vers domaine).

`nslookup` est présent autant sur Windows que sur Linux.
### Syntaxe d'utilisation
```
nslookup <nom_domaine> [serveur_dns]
nslookup <adresse_IP>
```
### Exemples d'utilisations
```bash
# Va envoyer une requête de résolution DNS vers le serveur DNS par défaut
# pour le domaine www.example.com (on veut l'adresse IP associée à www.example.com)
nslookup www.example.com 

# Va effectuer une requête DNS inverse (on veut le domaine associé à 8.8.8.8)
nslookup 8.8.8.8

# Va envoyer une requête de résolution DNS pour www.example.com vers le serveur 
# DNS se situant au 1.1.1.1 (on veut l'adresse IP associé à www.example.com)
nslookup www.example.com 1.1.1.1
```
