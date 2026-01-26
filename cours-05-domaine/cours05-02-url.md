# Qu'est-ce qu'un URL

## Définition

Un URL (Uniform Resource Locator) est une adresse standardisée qui permet d'identifier et de localiser une ressource sur Internet. C'est essentiellement l'adresse complète d'une page web, d'un fichier, d'une image ou de tout autre contenu accessible via le réseau.

Le terme URL signifie "Localisateur de Ressource Uniforme" et fait partie de la famille des URI (Uniform Resource Identifier).

## Rôle

Le rôle principal d'un URL est de:
- **Localiser précisément** une ressource sur Internet ou un réseau local
- **Spécifier le protocole** à utiliser pour accéder à la ressource
- **Identifier l'hôte** (serveur) qui héberge la ressource
- **Indiquer le chemin** vers la ressource spécifique sur ce serveur

L'URL permet à un client (navigateur, application) de savoir exactement où et comment accéder à une ressource donnée.

Les URLs sont utilisés par:
- **Navigateurs web** (Chrome, Firefox, Edge, Safari) pour accéder aux pages web
- **Clients de messagerie** pour accéder à des serveurs email via des protocoles web
- **Clients FTP** pour télécharger ou téléverser des fichiers
- **Certains utilitaires de ligne de commande** pour identifier une resource sur laquelle intéragir (e.g. **cUrl**)
- ...

## Les composants d'un URL

Un URL complet suit la structure suivante:
```
protocole://hôte:port/chemin?paramètres#ancre
```

<img src="img/Pasted image 20260126093139.png" width="500" />

### Le protocole

#### Rôle et syntaxe

Le protocole définit **comment** la ressource doit être récupérée. Il indique au client quelle méthode de communication utiliser pour accéder à la ressource.

**Syntaxe**: Le protocole est suivi de `://` (deux-points et deux barres obliques)

Exemple: `https://`, `ssh://`, `ftp://`

#### Quelques exemples communs

- **http** (HyperText Transfer Protocol) - Protocole web standard non sécurisé
- **https** (HTTP Secure) - Version sécurisée du protocole HTTP avec chiffrement SSL/TLS
- **ftp** (File Transfer Protocol) - Protocole de transfert de fichiers
- **mailto** - Pour créer des liens vers des adresses email
- **file** - Pour accéder à des fichiers locaux
- **ssh** - Pour les connexions sécurisées à distance
- **telnet** - Pour les connexions terminal à distance

### L'hôte

#### Rôle et syntaxe

L'hôte identifie **où** se trouve la ressource. Il spécifie le serveur qui héberge la ressource que l'on souhaite accéder.

**Syntaxe**: L'hôte suit immédiatement le protocole et peut être:
- Un nom de domaine (ex: `www.google.ca`)
- Une adresse IP (ex: `192.168.1.1`)

#### Types d'identifiants d'hôte acceptables

1. **Nom de domaine complet (FQDN)**
   - Exemple: `www.cegepal.ca`
   - Exemple: `mail.google.com`

2. **Adresse IPv4**
   - Exemple: `142.250.217.46`
   - Format: quatre octets séparés par des points

3. **Adresse IPv6**
   - Exemple: `[2607:f8b0:4006:81a::200e]`
   - Format: encadré par des crochets dans un URL

4. **Localhost**
   - `localhost` - Référence à la machine locale
   - `127.0.0.1` - Adresse IP de loopback

#### Quelques exemples communs

- `www.google.ca` - Moteur de recherche Google Canada
- `github.com` - Plateforme de développement collaboratif
- `192.168.0.1` - Adresse IP typique d'un routeur domestique
- `localhost` - Machine locale
- `claurendeau.qc.ca` - Site du Cégep André-Laurendeau

### Le port

#### Rôle et syntaxe

Le port spécifie **quel service** sur le serveur hôte doit traiter la requête. Chaque service réseau écoute sur un port spécifique.

**Syntaxe**: Le port est précédé de deux-points `:` après l'hôte

Exemple: `:8080`, `:443`, `:80`

#### Composant optionnel

Le port est **optionnel** dans un URL car chaque protocole possède un **port par défaut**:
- HTTP utilise le port **80** par défaut
- HTTPS utilise le port **443** par défaut
- FTP utilise le port **21** par défaut
- SSH utilise le port **22** par défaut

Si le port n'est pas spécifié, le navigateur ou l'application utilisera automatiquement le port par défaut du protocole.

**Exemples**:
- `http://example.com` → utilise automatiquement le port 80
- `https://example.com` → utilise automatiquement le port 443
- `http://example.com:8080` → utilise explicitement le port 8080

### Le chemin

#### Rôle et syntaxe

Le chemin indique **quelle ressource spécifique** on souhaite accéder sur le serveur hôte. Il représente l'emplacement du fichier ou de la page sur le serveur.

**Syntaxe**: 
- Commence par une barre oblique `/`
- Peut contenir plusieurs segments séparés par des barres obliques
- Peut se terminer par un nom de fichier avec extension

**Exemples**:
- `/index.html` - Page d'accueil
- `/images/logo.png` - Image dans le dossier images
- `/api/users/123` - Endpoint d'API pour l'utilisateur 123
- `/cours/informatique/reseau` - Hiérarchie de dossiers

**Composants additionnels** (optionnels):
- **Paramètres de requête** (query string): `?param1=valeur1&param2=valeur2`
- **Ancre** (fragment): `#section2` pour pointer vers une section spécifique de la page

# Quelques exemples d'url

Voici des exemples d'URLs complets avec leurs composants décomposés:

## Exemple 1: URL simple
```
https://www.cegepal.ca/index.html
```
- **Protocole**: https
- **Hôte**: www.cegepal.ca
- **Port**: 443 (par défaut, non spécifié)
- **Chemin**: /index.html

## Exemple 2: URL avec port explicite
```
http://192.168.1.100:8080/admin/dashboard
```
- **Protocole**: http
- **Hôte**: 192.168.1.100
- **Port**: 8080 (explicite)
- **Chemin**: /admin/dashboard

## Exemple 3: URL avec paramètres de requête
```
https://www.google.ca/search?q=reseau+informatique&lang=fr
```
- **Protocole**: https
- **Hôte**: www.google.ca
- **Port**: 443 (par défaut)
- **Chemin**: /search
- **Paramètres**: q=reseau+informatique&lang=fr

## Exemple 4: URL avec ancre
```
https://fr.wikipedia.org/wiki/Internet#Histoire
```
- **Protocole**: https
- **Hôte**: fr.wikipedia.org
- **Port**: 443 (par défaut)
- **Chemin**: /wiki/Internet
- **Ancre**: #Histoire

## Exemple 5: URL FTP
```
ftp://ftp.example.com:21/public/fichiers/document.pdf
```
- **Protocole**: ftp
- **Hôte**: ftp.example.com
- **Port**: 21 (explicite, même si c'est le port par défaut)
- **Chemin**: /public/fichiers/document.pdf

## Exemple 6: URL localhost avec port de développement
```
http://localhost:3000/api/users
```
- **Protocole**: http
- **Hôte**: localhost
- **Port**: 3000
- **Chemin**: /api/users

<hr>

[Précédent](cours05-01-nom-hote-domaine.md) - [Suivant](cours05-03-exercices-domaines-urls.md)