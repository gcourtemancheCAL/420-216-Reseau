# La commande `dhcpcd`

`dhcpcd` est un client DHCP (Dynamic Host Configuration Protocol) qui permet d'obtenir automatiquement une configuration IP depuis un serveur DHCP. 

`dhcpcd` va monitorer le statut d'une interface et gérer les paramètres DHCP de cette interface automatiquement. Le processus `dhcpcd` reste en vie tant et aussi longtemps qu'il a des interfaces à gérer.

## Obtenir une configuration DHCP

**Lancer `dhcpcd` pour une interface :**
```bash
sudo dhcpcd -4 eno1
```

**Lancer `dhcpcd` sur toutes les interfaces detectees :**
```bash
sudo dhcpcd -4
```

**Mode verbeux (utile pour le debogage) :**
```bash
sudo dhcpcd -4 -d eno1
```

## Relacher et renouveler le bail

**Relacher une adresse DHCP :**
```bash
sudo dhcpcd -k eno1
```

**Relacher toutes les adresses DHCP :**
```bash
sudo dhcpcd -k
```

**Renouveler une adresse DHCP :**
```bash
sudo dhcpcd -n eno1
```

## Consulter les baux DHCP

Les baux sont generalement stockes dans :

```bash
/var/lib/dhcpcd/
```

Ils vont prendre un nom similaire à :  `/var/lib/dhcpcd/dhcpcd-eno1.lease`

# Important : Persistence des manipulations

**ATTENTION : Le processus `dhcpcd` va continuer à gérer les adresses IP d'une interface tant qu'il est actif !**

Si vous voulez gérer votre interface manuellement, il peut être intéressant de relâcher les interfaces.

## Documentation supplémentaire : 

[Archwiki - dhcpcd](https://wiki.archlinux.org/title/Dhcpcd)
