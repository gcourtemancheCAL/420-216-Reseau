# Exercices pratiques - Noms de domaine et URLs

## Introduction

Ces exercices vous permettront de pratiquer les concepts de noms de domaine, de résolution DNS et de manipulation d'URLs en ligne de commande. Certains exercices sont spécifiques à Windows, d'autres à Linux, et certains sont disponibles sur les deux systèmes.

---

## Partie 1 : Résolution DNS avec nslookup

### Exercice 1.1 : Résolution simple (Windows/Linux)

**Objectif :** Résolution DNS de base

**Tâches :**
1. Utilisez `nslookup` pour trouver l'adresse IP de `www.google.com`
2. Utilisez `nslookup` pour trouver l'adresse IP de `github.com`
3. Utilisez `nslookup` pour trouver l'adresse IP de `claurendeau.qc.ca`

**Questions :**
- Quelle est l'adresse IP retournée pour chacun de ces domaines?
- Quel serveur DNS a répondu à votre requête?
- Certains domaines retournent-ils plusieurs adresses IP? Pourquoi?

### Exercice 1.2 : Résolution avec serveur DNS spécifique (Windows/Linux)

**Objectif :** Interroger différents serveurs DNS

**Tâches :**
1. Résolvez `www.example.com` en utilisant le serveur DNS de Google (`8.8.8.8`)
2. Résolvez `www.reddit.com` en utilisant le deuxième serveur DNS de Google (`8.8.4.4`)
3. Résolvez les mêmes domaine avec le serveur DNS de Cloudflare (`1.1.1.1`)
 
**Questions :**
- Les résultats sont-ils identiques entre les différents serveurs DNS?

### Exercice 1.3 : Résolution DNS inverse (Windows/Linux)

**Objectif :** Utiliser la résolution inverse pour trouver le nom de domaine associé à une IP

**Tâches :**
1. Effectuez une résolution inverse de l'adresse `8.8.8.8`
   ```
   nslookup 8.8.8.8
   ```
2. Effectuez une résolution inverse de `1.1.1.1`
3. Trouvez l'IP de `www.google.com` puis effectuez une résolution inverse de cette IP

**Questions :**
- Quel est le nom de domaine associé à `8.8.8.8`?
- Est-ce que la résolution inverse de l'IP de Google vous retourne exactement le même nom que vous avez utilisé initialement?

---

## Partie 2 : Gestion du cache DNS (Windows)

### Exercice 2.1 : Afficher le cache DNS (Windows)

**Objectif :** Visualiser les entrées en cache

**Tâches :**
1. Visitez quelques sites web dans votre navigateur (ex: google.com, github.com, reddit.com)
2. Affichez le contenu du cache DNS :
   ```powershell
   ipconfig /displaydns
   ```
3. Observez les entrées dans le cache

**Questions :**
- Combien d'entrées voyez-vous dans le cache?
- Pouvez-vous identifier les domaines que vous avez visités?
- Quelle est la durée de vie (TTL) des enregistrements? En quoi est-ce utile?

### Exercice 2.2 : Vider le cache DNS (Windows)

**Objectif :** Comprendre l'impact de la cache sur la résolution DNS. 

**Mentions particulières** : 
- Nous allons utiliser ping pour ces exercices puisque nslookup va toujours effectuer une requête dns (i.e. la cache ne sera pas utilisée)
- De nos jours, les serveur DNS sont tellement rapides et la latence tellement basse qu'il est difficile d'évaluer le temps de réponse. Afin de visualiser le temps réel utilisé par nos commandes, nous allons utiliser la commande powershell "Measure-Command"
- Les arguments que nous passons à `ping` servent à faire _échouer_ la commande le plus rapidement possible. Nous voulons évaluer le temps supplémentaire imposé par une requête DNS et non pas évaluer le temps de traitement par le serveur. 

**Tâches :**

1. Exécutez la commande suivante sur powershell et notez le temps d'exécution,
```powershell
# Measure-Command va nous donner le temps d'exécution d'une commande
# Les arguments de ping servent à faire échouer la commande le plus rapidement possible : 
#     L'argument `-n 1` de ping va limiter l'opération à l'envoie d'un seul paquet 
#     ICMP
#     L'argument `-w 1` va interrompre le processus d'attente de réponse après 1ms.
#     L'argument `-i 1` va faire en sorte que le ping échoue après 1 seul bond.
Measure-Command -Expression { ping -i 1 -n 1 -w 1 www.ujjivansfb.bank.in }
```

2. Exécutez la commande précédente une seconde fois notez le temps d'exécution. **Remarquez-vous une différence?**

3. Videz la cache DNS :
   ```powershell
   ipconfig /flushdns
   ```

4. Effectuez à nouveau la même commande et notez le temps d'exécution. **Remarquez-vous une différence?**

5. Affichez le cache DNS pour confirmer qu'il contient maintenant cette entrée

**Questions :**
- Pourquoi vider le cache DNS peut-il être utile en dépannage réseau?

---

## Partie 3 : Manipulation du fichier hosts

### Exercice 3.1 : Lecture du fichier hosts (Windows)

**Objectif :** Explorer le contenu du fichier hosts

**Tâches :**
1. Ouvrez le fichier hosts en lecture seule :
   ```powershell
   notepad C:\Windows\System32\drivers\etc\hosts
   ```

**Questions :**
- Quelle est l'entrée pour `localhost`?
- Y a-t-il d'autres entrées configurées?

### Exercice 3.2 : Modifier le fichier hosts (Windows - Administrateur requis)

**Objectif :** Créer une résolution DNS locale statique

**Tâches :**
1. Ouvrez l'application Bloc-notes (ou notepad++) en tant qu'administrateur
2. Ouvrez le fichier `C:\Windows\System32\drivers\etc\hosts`
3. Ajoutez une entrée pour créer un alias local :
   ```
   127.0.0.1    montest.local
   ```
4. Sauvegardez le fichier
5. Testez la résolution :
   ```powershell
   nslookup montest.local
   ping montest.local
   ```

**Questions :**
- Le `ping` fonctionne-t-il vers `montest.local`?
- Que se passe-t-il avec `nslookup montest.local`? Pourquoi?
- Ouvrez un navigateur et allez à `http://montest.local` - que se passe-t-il?

### Exercice 3.3 : Bloquer un domaine avec hosts (Windows)

**Objectif :** Utiliser le fichier hosts pour bloquer l'accès à un site

**Tâches :**
1. Ajoutez cette entrée dans le fichier hosts :
```
0.0.0.0    www.google.com
```
2. Essayez d'accéder à `www.google.com` dans votre navigateur
3. Retirez l'entrée et testez à nouveau

**Questions :**
- Pourquoi rediriger vers `0.0.0.0` bloque l'accès au site?
- Quels sont les cas d'utilisation pratiques de cette technique?

### Exercice 3.4 : Fichier hosts sous Linux

**Objectif :** Manipuler le fichier hosts sous Linux

**Tâches :**
1. Affichez le contenu du fichier hosts :
   ```bash
   cat /etc/hosts
   ```
2. Éditez le fichier (nécessite sudo) :
   ```bash
   sudo nano /etc/hosts
   ```
3. Ajoutez une entrée :
   ```
   127.0.0.1    montest.linux.local
   ```
4. Testez avec ping :
   ```bash
   ping montest.linux.local
   ```

---

## Partie 4 : Configuration DNS sous Linux

### Exercice 4.1 : Consulter la configuration DNS (Linux)

**Objectif :** Comprendre la configuration DNS sous Linux

**Tâches :**
1. Affichez le contenu de `/etc/resolv.conf` :
   ```bash
   cat /etc/resolv.conf
   ```
2. Identifiez les serveurs DNS configurés

**Questions :**
- Quels sont les serveurs DNS configurés sur votre système?

### Exercice 4.2 : Modifier la configuration DNS (Linux)

**Objectif :** Changer temporairement les serveurs DNS

**Tâches :**
1. Sauvegardez la configuration actuelle :
   ```bash
   sudo cp /etc/resolv.conf /etc/resolv.conf.backup
   ```
2. Éditez le fichier :
   ```bash
   sudo nano /etc/resolv.conf
   ```
3. Modifiez pour utiliser les DNS de Google :
   ```
   nameserver 8.8.8.8
   nameserver 8.8.4.4
   ```
4. Testez avec nslookup :
   ```bash
   nslookup www.google.com
   ```
5. Restaurez la configuration originale :
   ```bash
   sudo mv /etc/resolv.conf.backup /etc/resolv.conf
   ```

**Questions :**
- Selon-vous, quelles seraient les intéractions entre une configuration DHCP et le fichier `/etc/resolv.conf` ?
- Cette modification peut être temporaire sur certains systèmes. Pourquoi?

### Exercice 4.3 : Consulter l'ordre de résolution (Linux)

**Objectif :** Comprendre le processus de résolution sous Linux

**Tâches :**
1. Consultez le fichier de configuration de résolution :
   ```bash
   cat /etc/nsswitch.conf
   ```
2. Trouvez la ligne commençant par `hosts:`

**Questions :**
- Quel est l'ordre de résolution configuré sur votre système?
- Y a-t-il une configuration pour mDNS?

---

## Partie 5 : Tester les connexions avec curl (Windows/Linux)

### Exercice 5.1 : Récupérer une page web (Windows/Linux)

**Objectif :** Utiliser curl pour télécharger du contenu via URL

**Tâches :**
1. Installez curl si nécessaire (déjà inclus dans Windows 10/11 et la plupart des distributions Linux)
	1. **Sur linux** : `sudo apt install -y curl`

2. Récupérez la page d'accueil de Google :
   ```bash
   curl http://www.google.com
   ```

3. Sauvegardez le contenu dans un fichier :
   ```bash
   curl https://www.google.com -o page.html
   ```

**Questions :**
- Quelle est la différence de résultat entre "http" et "https"?
- Qu'est-ce que "http" et "https" représentent dans l'url?

### Exercice 5.2 : Tester des ports personnalisés (Windows/Linux)

**Objectif :** Comprendre l'utilisation des ports dans les URLs

**Tâches :**
1. Testez un port standard :
   ```bash
   curl http://www.google.com:80
   ```

2. Testez un port non standard :
   ```bash
   curl http://www.google.com:8080
   ```
### Exercice 5.3 : Connexion à l'adresse ip

#### Exercice 5.3.1 - Cas de figure normal

**Objectif :** Comprendre les formes que peuvent prendre l'hôte dans l'url et comprendre la différence entre domaine et url.

1. En utilisant votre navigateur web, connectez-vous au `http://www.midwinter.com/lurk/`. Prenez un instant pour admirer le look sublime de la page!
2. À l'aide de `nslookup`, identifiez l'adresse IP associé au domaine.
3. Connectez à ce même site en utilisant directement son adresse IP dans l'url.

**N.B :** Vous devriez attérir sur la même page.

#### Exercice 5.3.2 - Cas de figure spécial

Répetez la procédure précédente, mais cette fois ci pour l'url `http://www.example.com/`

**Question** : Est-ce que vous arrivez sur la même page? Qu'est-ce qui explique le résultat?

### Exercice 5.4 : URL et domaine

**Objectif :** Comprendre la différence entre un url et un domaine comme argument à une commande.

1. Utilisez la commande `ping` afin de rejoindre `www.google.com`
2. Utilisez la commande `nslookup` afin de résoude le `www.google.com`
3. Répetez ces deux étapes avec l'url `https://www.google.com`

**Question :** Expliquez pourquoi les commandes ne fonctionnent pas lorsqu'un url est utilisé. 

---

## Partie 6 : Analyse réseau avec ping

### Exercice 6.1 : Ping vers nom de domaine (Windows/Linux)

**Objectif :** Tester la connectivité en utilisant des noms de domaine

**Tâches Windows :**
```powershell
ping www.google.com
ping -n 10 www.github.com
ping -t www.reddit.com  # (Ctrl+C pour arrêter)
```

**Tâches Linux :**
```bash
ping www.google.com # (Ctrl+C pour arrêter)
ping -c 10 www.github.com
```

**Questions :**
- Quelle IP est résolue pour chaque domaine?
- À quel information correspond le temps associé à chaque tentative de ping?
- Y a-t-il des pertes de paquets?

## Partie 7 : Analyse réseau avec ping

---

## Ressources supplémentaires

### Commandes rapides de référence

**Windows :**
```powershell
ipconfig /all                          # Configuration complète
ipconfig /flushdns                     # Vider le cache DNS
ipconfig /displaydns                   # Afficher le cache DNS
nslookup [domaine]                     # Résolution DNS
Resolve-DnsName [domaine]              # Résolution DNS PowerShell
ping [domaine]                         # Test ICMP
curl [url]                             # Récupérer une ressource
```

**Linux :**
```bash
cat /etc/resolv.conf          # Configuration DNS
cat /etc/hosts                # Fichier hosts
cat /etc/nsswitch.conf        # Ordre de résolution
nslookup [domaine]            # Résolution DNS
ping [domaine]                # Test ICMP
curl [url]                    # Récupérer une ressource
```

### Sites web utiles pour les tests

- `www.example.com` - Site de test officiel
- `www.google.com` - Toujours accessible

<hr>

[Précédent](cours05-02-url.md)