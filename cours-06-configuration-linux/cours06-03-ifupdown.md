# `ifup` et `ifdown`

Les commandes `ifup` et `ifdown` sont des outils traditionnels sous Debian/Ubuntu pour gérer les interfaces réseau de manière persistante. Contrairement aux commandes `ip` qui modifient temporairement la configuration, `ifup` et `ifdown` utilisent le fichier de configuration `/etc/network/interfaces`.

## Comment ça marche

### En général

Le système `ifupdown` fonctionne selon ce principe :

1. **Fichier de configuration** : `/etc/network/interfaces` contient la configuration de toutes les interfaces
2. **Commandes de contrôle** : `ifup` active une interface, `ifdown` la désactive
3. **État persistant** : les configurations sont appliquées au démarrage et persistent donc après le redémarrage

### Démarrage de l'ordinateur

Au démarrage, le système Debian :

1. **Lecture du fichier** `/etc/network/interfaces`
2. **Activation automatique** des interfaces marquées avec `auto`
3. **Détection hotplug** pour les interfaces marquées avec `allow-hotplug`

**Service système responsable :**
```bash
systemctl status networking.service
```

Ce service exécute essentiellement :
```bash
ifup -a
```

Ce qui active toutes les interfaces configurées avec `auto`.

## Rôle des commandes

### `ifup`

La commande `ifup` **active** (bring up) une interface réseau en appliquant la configuration définie dans `/etc/network/interfaces`.

**Syntaxe de base :**
```bash
sudo ifup <interface>
```

**Exemple :**
```bash
sudo ifup eno1
```

**Ce que fait `ifup` :**
1. Lit la configuration de l'interface dans `/etc/network/interfaces`
2. Active l'interface au niveau liaison (équivalent de `ip link set up`)
3. Configure l'adresse IP (statique ou via DHCP)
4. Configure les routes associées
5. Exécute les scripts pré/post-up s'ils sont définis

### `ifdown`

La commande `ifdown` **désactive** (bring down) une interface réseau.

**Syntaxe de base :**
```bash
sudo ifdown <interface>
```

**Exemple :**
```bash
sudo ifdown eno1
```

**Ce que fait `ifdown` :**
1. Exécute les scripts pre-down s'ils sont définis
2. Supprime les routes associées à l'interface
3. Supprime l'adresse IP de l'interface
4. Désactive l'interface au niveau liaison
5. Exécute les scripts post-down s'ils sont définis

## Le fichier `/etc/network/interfaces`

Le fichier `/etc/network/interfaces` est le fichier de configuration central pour les interfaces réseau sous Debian.

**Structure de base :**
```bash
# Interface de loopback
auto lo
iface lo inet loopback
```

### Identifier une interface

Une interface est définie par la ligne `iface` :

```bash
iface <nom_interface> <famille_adresse> <méthode>
```

**Paramètres :**
- `<nom_interface>` : nom de l'interface (eth0, enp0s3, wlan0, etc.)
- `<famille_adresse>` : `inet` (IPv4) ou `inet6` (IPv6)
- `<méthode>` : `static`, `dhcp`, `loopback`, `manual`

### Configuration statique

Pour configurer une adresse IP statique :

```bash
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

**Ou avec notation CIDR (plus moderne) :**
```bash
auto eth0
iface eth0 inet static
    address 192.168.1.100/24
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

**Paramètres disponibles :**
- `address` : adresse IP de l'interface
- `netmask` : masque de sous-réseau (ou notation CIDR dans address)
- `gateway` : passerelle par défaut
- `dns-nameservers` : serveurs DNS (séparés par des espaces). Notez bien que cette option nécessite l'installation du package `resolvconf`
- `network` : adresse réseau (calculée automatiquement si omise)

**Scripts personnalisés :**
```bash
iface eth0 inet static
    address 192.168.1.100/24
    gateway 192.168.1.1
    pre-up echo "Activation de eth0..."
    post-up /usr/local/bin/mon-script.sh
    pre-down echo "Désactivation de eth0..."
    post-down /usr/local/bin/nettoyage.sh
```

### Configuration dynamique

Pour obtenir une configuration automatique via DHCP :

```bash
auto eth0
iface eth0 inet dhcp
```

**C'est tout !** Le système utilisera `dhclient` automatiquement.

### `auto`

La directive `auto` indique qu'une interface doit être activée automatiquement au démarrage du système.

```bash
auto eth0
iface eth0 inet dhcp
```

**Comportement :**
- L'interface est activée par `ifup -a` au boot
- Exécutée par le service `networking.service`
- Idéal pour les interfaces câblées permanentes

**Multiple interfaces :**
```bash
auto eth0
auto eth1

iface eth0 inet static
    address 192.168.1.100/24

iface eth1 inet dhcp
```

### `allow-hotplug`

La directive `allow-hotplug` indique qu'une interface doit être activée automatiquement lorsqu'elle est détectée physiquement (branchée).

```bash
allow-hotplug eno1
iface eno1 inet dhcp
```

**Exemple pratique :**
```bash
# Interface Ethernet câblée - toujours présente
auto eth0
iface eth0 inet dhcp

# Interface WiFi - peut être désactivée ou absente
allow-hotplug wlan0
iface wlan0 inet dhcp
```

## Comment utiliser les commandes ifup/ifdown

### Identifier les interfaces

#### Par son nom

La méthode la plus directe : spécifier le nom de l'interface.

```bash
# Activer une interface
sudo ifup eth0

# Désactiver une interface
sudo ifdown eth0
```

**Redémarrer une interface (appliquer des changements) :**
```bash
sudo ifdown eth0 && sudo ifup eth0
```

**Afficher ce qui se passe (mode verbeux) :**
```bash
sudo ifup -v eth0
```

#### `--all`

L'option `--all` (ou `-a`) traite toutes les interfaces marquées avec `auto`.

```bash
# Activer toutes les interfaces auto
sudo ifup -a

# Désactiver toutes les interfaces auto
sudo ifdown -a
```

**Usage typique :**
- Au démarrage du système
- Pour réinitialiser toute la configuration réseau

**Exemple :**
```bash
# Redémarrer toute la configuration réseau
sudo ifdown -a && sudo ifup -a
```

Ou avec le service système :
```bash
sudo systemctl restart networking.service
```

#### `--allow=hotplug`

L'option `--allow=` permet de cibler un groupe spécifique d'interfaces.

```bash
# Activer toutes les interfaces allow-hotplug
sudo ifup --allow=hotplug

# Désactiver toutes les interfaces allow-hotplug
sudo ifdown --allow=hotplug
```

## Considération particulières

Le système `ifupdown` maintient un état interne de ce qu'il pense être l'état des interfaces. Cet état est stocké dans `/run/network/ifstate`.

**Problème potentiel :**
Si vous modifiez manuellement une interface avec `ip` ou `dhclient`, l'état réel peut différer de ce que `ifupdown` pense.

**Exemple de problème :**
```bash
# Vous configurez manuellement eth0
sudo ip address add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up

# ifupdown pense que eth0 est DOWN
# Essayer de l'activer avec ifup échouera
sudo ifup eth0
# Erreur: ifup: interface eth0 already configured
```

**Autre scénario problématique :**
```bash
ifup eno1

# On modifie la configuration de eno1
vim /etc/network/interfaces

ifdown eno1
```

### `--force`

L'option `--force` permet de forcer l'exécution même si `ifupdown` pense que l'interface est déjà dans l'état demandé.

```bash
# Forcer l'activation même si marquée comme UP
sudo ifup --force eth0

# Forcer la désactivation même si marquée comme DOWN
sudo ifdown --force eth0
```


