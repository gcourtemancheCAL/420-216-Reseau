# La commande `ip`

La commande `ip` est l'outil moderne sous Linux pour gérer les interfaces réseau, les adresses IP et le routage. Elle remplace les anciennes commandes comme `ifconfig`, `route` et `arp`.

## La structure d'une commande ip

La commande `ip` suit une structure générale :

```bash
ip [OPTIONS] OBJET COMMANDE
```

**Les objets principaux :**
- `address` (ou `addr` ou `a`) : gestion des adresses IP
- `link` (ou `l`) : gestion des interfaces réseau
- `route` (ou `r`) : gestion des routes
- `neighbour` (ou `neigh` ou `n`) : gestion de la table ARP

**Options courantes :**
- `-4` : afficher uniquement IPv4
- `-6` : afficher uniquement IPv6
- `-c` : utiliser des couleurs dans l'affichage
- `-br` : format bref (brief)
- `-s` : statistiques

## Afficher de l'information

### ip address

La commande `ip address` (ou `ip addr` ou `ip a`) affiche les adresses IP configurées sur toutes les interfaces :

```bash
ip address show
```

**Exemple de sortie :**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
    link/ether 08:00:27:3f:84:a1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
    inet6 fe80::a00:27ff:fe3f:84a1/64 scope link
```

**Pour afficher une interface spécifique :**
```bash
ip address show eth0
ip a show dev eth0
```

**Format bref :**
```bash
ip -br addr
```

### ip link

La commande `ip link` affiche l'état des interfaces réseau au niveau liaison (couche 2) :

```bash
ip link show
```

**Exemple de sortie :**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
    link/ether 08:00:27:3f:84:a1 brd ff:ff:ff:ff:ff:ff
```

**Informations affichées :**
- État de l'interface (UP/DOWN)
- Adresse MAC
- MTU (Maximum Transmission Unit)
- État de la liaison physique

### ip route

La commande `ip route` affiche les informations de routing de votre système. Sur un hôte Linux standard, la seule information utile s'y trouvant est la passerelle par defaut.

<img src="img/Pasted image 20260202092820.png" width="500" />

**Retirer la route par défaut :**

```bash
sudo ip route del default

# Ou plus explicitement : 
sudo ip route del default via 192.168.18.1 dev enp0s3
```

**Ajouter une route par défaut :**

```bash
# NB : Ne va pas fonctionner si une route par défaut existe déjà
sudo ip route add default via 192.168.18.1
```

**Remplacer la route par défaut :**

```bash
sudo ip route replace default via 192.168.18.1
```

## Activer/Désactiver une interface

**Activer une interface :**
```bash
sudo ip link set eno1 up
# ou plus explicitement
sudo ip link set up dev eno1
```

**Désactiver une interface :**
```bash
sudo ip link set eno1 down
# ou plus explicitement
sudo ip link set down dev eno1
```

**Note :** Ces commandes nécessitent les privilèges root (d'où l'utilisation de `sudo`).

## Modifier l'adresse IP

### Ajouter une adresse IP

Pour ajouter une adresse IP à une interface :

```bash
sudo ip address add 192.168.1.100/24 dev eno1
```

**Ajouter une adresse secondaire :**
Une interface peut avoir plusieurs adresses IP :
```bash
sudo ip address add 192.168.1.101/24 dev eno1
```

### Retirer une adresse IP

Pour supprimer une adresse IP spécifique :

```bash
sudo ip address del 192.168.1.100/24 dev eno1
```

**Pour supprimer toutes les adresses d'une interface :**
```bash
sudo ip address flush dev eno1
```

# Important : Persistence des manipulations

**ATTENTION : Les modifications effectuées avec les commandes `ip` ne sont PAS persistantes !**

**Ce que cela signifie :**
- Les paramètres IP modifiés avec `ip address add/del` sont perdus au redémarrage
- Toutes les modifications sont temporaires et n'existent que dans l'exécution actuelle du système

**Pourquoi ce comportement ?**
Ces commandes modifient directement la configuration du noyau Linux en mémoire, mais ne touchent pas aux fichiers de configuration système.

**Cas d'utilisation des commandes temporaires :**
- Tests rapides de connectivité
- Dépannage réseau
- Configuration temporaire pour une tâche spécifique
- Environnements de laboratoire où les configurations changent fréquemment
- Correctif en cours de configuration

**Exemple pratique :**
```bash
# Configuration temporaire pour un test
sudo ip address add 192.168.10.50/24 dev eno1

# Après le test, retour à la normale au prochain redémarrage
sudo reboot
```