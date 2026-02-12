# Atelier formatif — Configuration IP avec `ifup`/`ifdown`

## Objectif général

Mettre en pratique la configuration réseau **persistante** sous Debian à l’aide de `ifup`, `ifdown` et du fichier `/etc/network/interfaces`

Cet atelier peut être réalisé seul ou en groupe. 

**Si vous réalisé l'atelier en groupe :** assurez-vous de travailler de façon à ce que tous les membres de l'équipe puisse pratiquer la matière - vous devrez (tous et chacun) être capable d'effectuer ces manipulations **sans référence** rendu en évaluation.

**À la fin de cet atelier vous devrez** :
- Être capable de définir la configuration statique et dynamique d'une interface dans le fichier `interfaces`
- Être capable d'utiliser les commandes `ifup` et `ifdown` afin de recharger la configuration d'une interface.
- Comprendre le modèle logique maintenu par les commandes `ifup` et `ifdown`
- 
## Mise en place

*N.b. Cet atelier peut être réalisé immédiatement suivant l'atelier 1 - redémarrer l'ordinateur devrait vous mettre dans l'état initial attendu.*

Cet atelier est à réaliser sur une machine virtuelle sur laquelle vous aurez préalablement installer Debian. Des instructions spécifiques vous ont été fournies à la séance précédente. 

**Instructions de mise en place :**
- Connectez, par câble, votre ordinateur portable au routeur mis à votre disposition.
- Configurez votre machine virtuelle en mode réseau "par pont" ou "bridged adapter". Vous devrez - à cet étape - sélectionner l'adaptateur physique avec lequel vous allez effectuer un pont. Sélectionnez votre adaptateur ethernet.
- Démarrez votre machine virtuelle.
- Une fois la machine virtuelle démarrée, testez la connectivité au réseau à l'aide de la commande `ping -c 4 192.168.100.1`

---

## Partie A — État initial et sauvegarde

### A1 — Sauvegarder la configuration actuelle
1. Créez une copie du fichier `/etc/network/interfaces` dans votre home. Cette copie ne servira simplement qu'à restaurer le fichier à son état initial en cas de problème.

---

### A2 — Lecture de la configuration initiale
1. Affichez et lisez le contenu du fichier `/etc/network/interfaces`.

**Questions**
- Quels interfaces y sont configurés?
- Identifiez la configuration de l'interface de loopback.
- Identifiez la configuration de votre interface Ethernet. 
	- Est-ce une configuration statique ou dynamique?
	- Quelle ligne fait en sorte que votre interface est configurée au démarrage de l'ordinateur?

---

## Partie B — Configuration persistante

### B1 — Éditer `/etc/network/interfaces`
1. Désactivez votre interface à l'aide la commande `ifdown`
	1. Validez à l'aide des commandes `ip`
2. Modifiez la configuration de l’interface active pour une **adresse statique**.
3. Utilisez une adresse entre le `192.168.100.100` et le `192.168.10.200`. Synchronisez vous avec vos voisins de routeur afin d'éviter que vous utilisiez la même adresse.
4. Activez la configuration à l'aide de `ifup`.
	1. Validez à l'aide des commandes `ip`

Vous devriez être en mesure de rejoindre le routeur et internet (e.g. `8.8.8.8`).

**Exemple :**
```bash
auto enp0s3
iface enp0s3 inet static
    address 192.168.100.101/24
    gateway 192.168.100.1
```

### B2 — Configuration DNS

1. Essayez de rejoindre le `www.google.com` avec `ping`.
2. Qu'est-ce qui se passe? Pourquoi?
3. Corrigez la situation en configurant le serveur DNS de Cloudflare (1.1.1.1) dans le fichier `/etc/resolv.conf`.

### B3 - Modifier le fichier pour DHCP
1. Désactivez votre interface à l'aide la commande `ifdown`
2. Remplacez la configuration statique par DHCP.
3. Activez la configuration à l'aide de `ifup`.
	1. Validez à l'aide des commandes `ip`

**Exemple :**
```bash
auto enp0s3
iface enp0s3 inet dhcp
```

**Questions**
1. Lorsque vous utilisez la commande `nslookup`, quel serveur DNS est utilisé?
2. Est-ce le même que vous avez configuré? Validez dans le fichier `resolv.conf`
3. D'où vient cette configuration?

### B3 - Configuration DNS statique via `interfaces`

Certaines options de configuration du fichier `interfaces` vont nécessiter l'installation de packages spécifiques. La configuration de serveur statique en est un exemple.

1. Installez le package `resolvconf`

```bash
sudo apt install resolvconf
```

2. Désactivez votre interface à l'aide de la commande `ifdown`
3.  Configurez statiquement votre interface mais cette fois-ci ajoutez la ligne `dns-nameservers 1.1.1.1`

**Exemple**

```bash
auto enp0s3
iface enp0s3 inet static
    address 192.168.100.101/24
    gateway 192.168.100.1
    dns-nameservers 1.1.1.1
```

4. Activez l'interface via `ifup`
5. Validez votre configuration ip.
6. Validez votre configuration DNS en utilisant la commande `nslookup`.


---

## Partie C — Auto vs allow-hotplug

**NB : Je ne suis pas super satisfait des exercices ici. Libre à vous de les sauter. Je conseillerais quand même de lire le paragraphe suivant qui explique la différence entre `auto` et `allow-hotplug`**

***N.B** : `auto` et  `allow-hotplug` vont tout deux faire en sorte que la configuration est appliquée au démarrage de l'ordinateur. La principale différence est que `allow-hotplug` est réactif au statut de connexion de l'interface, tandis que `auto` va forcer les paramètres au démarrage.* 

*Plusieurs nuances et subtilités sont impliquées à ce niveau. La configuration d'interfaces par défaut va préconiser l'utilisation de `allow-hotplug`.*

*Les deux options jouent un rôle similaire alors nous allons vouloir éviter de les combiner sur une même interface afin d'éviter les conflits étranges.*

### C1 — Comprendre `auto`
1. Assurez-vous que l’interface est définie avec `auto`.
2. Redémarrez la machine.
3. Vérifiez si l’interface est automatiquement active.
4. Exécutez la commande `ifdown -a`. Quelles interfaces ont été désactivées?
5. Exécutez la commande `ifup -a`. Quelles interfaces ont été activées?

---

### C2 — Tester `allow-hotplug`
1. Remplacez `auto` par `allow-hotplug`.
2. Redémarrez.
3. Vérifiez le comportement de l’interface au démarrage.
4. Exécutez la commande `ifdown -a`. Quelles interfaces ont été désactivées?
5. Exécutez la commande `ifup -a`. Quelles interfaces ont été activées?

---

## Partie E — Gestion d’état et `--force`

### E1 — Scénario d’état incohérent
1. Activez l’interface avec `ifup`.
2. Désactivez-la manuellement avec `ip link set <interface> down`.
3. Tentez de refaire `ifup`.

---

### E2 — Forcer la réactivation
1. Utilisez `ifdown --force <intf>` suivi de `ifup --force <intf>` pour corriger la situation.
2. Vérifiez l’état final.


---

## Partie F — Restauration

### F1 — Restaurer la configuration d’origine
1. Restaurez le fichier `/etc/network/interfaces` à partir de la sauvegarde.
2. Redémarrez le système

---

## Aide-mémoire

- Voir la config actuelle :
  - `cat /etc/network/interfaces`
- Appliquer la config :
  - `sudo ifdown <interface>`
  - `sudo ifup <interface>`
- Toutes les interfaces auto :
  - `sudo ifup -a`
  - `sudo ifdown -a`
- Hotplug :
  - `sudo ifup --allow=hotplug`
- Forcer :
  - `sudo ifup --force <interface>`
  - `sudo ifdown --force <interface>`
