# Atelier intégré — Configuration réseau Windows et Linux

## Objectif général
Cet atelier pratique combine la matière des cours 04, 05 et 06. Vous allez configurer un réseau simple composé d'un système Windows et d'un système Linux, mettre en pratique le calcul d'adresses réseau, les configurations statique et dynamique, et tester la connectivité avec des outils de diagnostic.

De façon plus spécifique, cet atelier vis à vous faire pratiquer un peu l'ensemble des éléments couverts jusqu'à présent.

Un autre objectif consiste à vous exposer à certains cas d'erreurs et de vous amener à utiliser les éléments théoriques couverts jusqu'à présent afin de comprendre et d'expliquer ces cas d'erreurs. Le tout se fait dans l'objectif de vous amener à développer l'aptitude d'identifier par vous-mêmes les problèmes que vous allez potentiellement rencontrer à l'avenir.

---

# Partie 1 — Connexion initiale au réseau

## 1.1 — Connecter l'ordinateur au routeur

1. Connecter votre ordinateur portable au routeur.
2. Assurez-vous que la configuration IP de son interface ethernet se fait via DHCP.
3. En utilisant la commande apropriée, renouveller l'adresse acquise via DHCP de votre interface Ethernet.

**Identifiez les paramètres IP suivant sur votre machine Windows pour votre interface active :**

- Adresse IP
- Masque réseau
- Adresse réseau
- Passerelle par défaut
- Serveur DNS

**Identifiez aussi l'adresse MAC de l'interface Ethernet active de votre système**

## 1.2 — Connecter la machine virtuelle au routeur

1. Démarrez la machine virtuelle
2. Assurez-vous que la configuration de votre machine virtuelle se fait par DHCP. Effectuez les manipulations apropriés si nécessaires.

**Identifiez les paramètres IP suivant sur votre machine virtuelle pour votre interface active :**

- Adresse IP
- Masque réseau
- Adresse réseau
- Passerelle par défaut
- Serveur DNS

**Identifiez aussi l'adresse MAC de l'interface Ethernet active de votre système**

## 1.3 — Validation de la connectivité

À cette étape, votre système Windows et votre machine virutelle Debian devraient pouvoir se rejoindre via ping.

Testez la connectivité entre vos deux systèmes. Si le test échoue, essayer d'identifier et de régler le problème par vous-même.

**Questions pouvant aider le dépannage :**
	- Est-ce que vos interfaces sont actives?
	- Est ce que vos paramètres IP ont été correctement assignés?
		- Les deux systèmes devraient être dans le même réseau
	- Est-ce que votre machine virtuelle est en mode "bridged adapter"?
	- Est-ce que vos deux systèmes sont capable de rejoindre le routeur?
	- Est-ce que le pare-feu permet autorise les messages ICMP entrant?

## 1.4 — Tests de connectivité internet

### 1.4.1 — Tester la résolution DNS

### Windows
```powershell
nslookup google.com
nslookup 8.8.8.8  # reverse lookup
```
### Linux
```bash
nslookup google.com
nslookup 8.8.8.8
```

**Identifiez le serveur DNS utilisé par Windows et Linux.**

### 1.4.2 — Tester la connexion vers internet

Vos deux systèmes devraient être en mesure de rejoindre l'internet. Essayez de rejoindres quelques domaines connus (google, le domaine du cégep, ...)

# Partie 2 — Configuration statique

### 2.1 — Configuration statique sur Windows

Changez la configuration IP de votre système Windows avec les paramètres suivants : 
- Adresse IP : `192.168.100.X/23`, où `X = 64 + (# port lan * 2)`
- Passerelle par défaut : `192.168.100.1`
- Serveur DNS : `8.8.8.8`

**Calculez les éléments suivants en utilisant le masque réseau de votre système Windows:** 
- Masque réseau de votre système
- L'adresse réseau de votre système
- L'adresse réseau du routeur

**Effectuez les tests suivants sur le système Windows. Expliquez les résultats:**
- Essayez de rejoindre le routeur
- Essayez de rejoindre la machine virtuelle Debian.
- Essayez de rejoindre le 8.8.8.8

*Les tests précédents devraient tous réussir.*

### 2.2 — Configuration statique sur Linux

**NB : Assurez-vous d'installer le package `resolvconf` afin de pouvoir configurer le DNS directement dans le fichier `interfaces`.**

À l'aide des utilitaires `ifup` et `ifdown`, changez la configuration IP de votre système Linux avec les paramètres suivants : 
- Adresse IP : `192.168.100.X/26`, où `X = 65 + (# port lan * 2)`
- Passerelle par défaut : `192.168.100.1`
- Serveur DNS : `8.8.8.8

**Calculez les éléments suivants en utilisant le masque réseau de votre machine virtuelle :** 
- Masque réseau de votre système
- L'adresse réseau du routeur
- L'adresse réseau du système Linux
- L'adresse réseau du système Windows

**Question** : En voyant ces paramètres, croyez-vous que votre système va être en mesure de rejoindre la passerelle?

**Effectuez les tests suivants sur la machine virtuelle. Expliquez les résultats:**
- Essayez de rejoindre le routeur
- Essayez de rejoindre système Windows.
- Essayez de rejoindre le 8.8.8.8

**Questions :** Est-ce que le résultat de ces tests corresponds à vos attentes?

**NOTE IMPORTANTE** : La configuration actuelle est problématique et pourrait causer des problèmes plus tard. Elle s'adonne à fonctionner dans notre situation mais il n'y a aucune garanti à cet effet. De façon générale, il est très important de s'assurer que notre système est dans le même réseau que sa passerelle.

### 2.3 — Linux #2

À l'aide de l'utilitaire `ip`, changez l'adresse IP de interface active sur Linux pour la suivante
- Adresse IP : `192.168.101.X/26`, où `X = 65 + (# port lan * 2)`

**Calculez les éléments suivants en utilisant le masque réseau de votre machine virtuelle :** 
- Masque réseau de votre système
- L'adresse réseau du routeur
- L'adresse réseau du système Windows

**Question** : En voyant ces paramètres, croyez-vous que votre système va être en mesure de rejoindre la passerelle?

**Effectuez les tests suivants sur la machine virtuelle. Expliquez les résultats:**
- Essayez de rejoindre le routeur
- Essayez de rejoindre système Windows.
- Essayez de rejoindre le 8.8.8.8

**EXPLICATIONS** : Le cas précédent fonctionnait parce que le routeur identifiait qu'on était dans le même réseau que lui en utilisant son propre masque réseau (qui peut être différent du notre).

Ce coup ci, le routeur va voir qu'on est dans un réseau different vers lequel il n'est pas capable de router. Ainsi, les réponses ne vont jamais se rendre à nous.

### 2.4 — Linux #3

À l'aide de l'utilitaire `ip`, changez l'adresse IP de interface active sur Linux pour la suivante
- Adresse IP : `192.168.100.X/26`, où `X = 65 + (# port lan * 2)`

À l'aide de l'utilitaire `ip`, supprimer la route par défaut.

**Effectuez les tests suivants sur la machine virtuelle :**
- Essayez de rejoindre le routeur
- Essayez de rejoindre système Windows.
- Essayez de rejoindre le 8.8.8.8

**Questions :**
- Normalement, vous devriez être de rejoindre uniquement le système Windows. Expliquez pourquoi vous pouvez rejoindre le système Windows et non pas les autres.

### 2.4 — Windows #2

Changez la configuration IP du système Windows par la suivante : 
- Adresse IP : `192.168.100.X/27`, où `X = 96 + (# port lan * 2)`
- Passerelle par défaut : `192.168.100.1`

**Calculez les éléments suivants en utilisant le masque réseau de votre système Windows:** 
- Masque réseau de votre système Windows
- L'adresse réseau de votre système Windows
- L'adresse réseau de votre passerelle
- L'adresse réseau de votre système Linux

**Effectuez les tests suivants sur la machine virtuelle :**
- Essayez de rejoindre le routeur
- Essayez de rejoindre système Windows.
- Essayez de rejoindre le 8.8.8.8

**Effectuez les tests suivants sur le système Windows :**
- Essayez de rejoindre le routeur
- Essayez de rejoindre système Windows.
- Essayez de rejoindre le 8.8.8.8

Expliquez vos résultats sur la base des éléments vu précédemment.

## Partie 3 — Configuration de nom d'hôte .local

### 3.1 — Configuration statique

Affectez la configuration statique suivante au système Windows : 
- Adresse IP : `192.168.100.X/24`, où `X = 100 + (# port lan * 2)`
- Passerelle par défaut : `192.168.100.1`
- Serveur DNS : `1.1.1.1`

En utilisant `ifup` et `ifdown`, affectez la configuration statique suivante à l'hôte Linux : 
- - Adresse IP : `192.168.100.X/24`, où `X = 101 + (# port lan * 2)`
- Passerelle par défaut : `192.168.100.1`
- Serveur DNS : `8.8.8.8`

Sur le système Windows, créez dans le fichier `hosts` une entrée assignant le nom d'hôte "debian.local" à l'adresse ip de votre machine virtuelle.

Sur le système Linux, créez dans le fichier `hosts` une entrée assignant le nom d'hôte "win.local" à l'adresse ip de votre système windows.

**Effectuez les tests suivants :**
- À partir du système Windows, essayez de rejoindre l'hôte `linux.local` à l'aide de ping.
- À partir de la machine virtuelle, essayez de rejoindre l'hôte `win.local` à l'aide de ping.

*Les tests précédents devraient fonctionner*

**Effectuez les tests suivants :**
- À partir du système Windows, essayez de rejoindre l'hôte `win.local` à l'aide de ping.
- À partir de la machine virtuelle, essayez de rejoindre l'hôte `linux.local` à l'aide de ping.

*Les tests précédents devraient échouer.* **Expliquez pourquoi.**

**Effectuez les test suivants :**
- Sur Windows, effectuez la commande nslookup `linux.local`. 
 
Obtenez-vous un résultat? Pourquoi?

**Effectuez les test suivants :**
- Sur Linux, exécutez la commande `nslookup win.local`.
- Sur Linux, exécutez la commande `getent ahostsv4 win.local` 

L'une de ses commandes va vous donner un résultat, et l'autre non. Pourquoi? Quelle est la différence entre ces deux commandes?

