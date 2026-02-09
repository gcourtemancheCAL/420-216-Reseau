# ICMP

## Description

ICMP (Internet Control Message Protocol) est un protocole utilitaire de la couche 3 utilisé pour rapporter les erreurs et envoyer des messages de diagnostic. Contrairement à TCP ou UDP, ICMP ne transporte pas de données applicatives.

ICMP est encapsulé dans les paquets IP (protocole IP numéro 1).

## Notifications d'erreurs

ICMP est principalement utilisé pour signaler des conditions d'erreur lors de l'acheminement des paquets.

### Destination Unreachable

Généré lorsqu'un routeur ou l'hôte de destination ne peut pas livrer un paquet. Les raisons incluent:
- Hôte injoignable
- Port fermé (destination port unreachable)
- Réseau injoignable
- Paquet fragmenté quand la fragmentation n'est pas autorisée

### Time Exceeded

Généré lorsque le TTL d'un paquet atteint zéro avant d'arriver à la destination ou lors du réassemblage des fragments.

## ping

`ping` utilise ICMP Echo Request et Echo Reply.

L'émetteur envoie un message Echo Request et l'hôte de destination répond avec un Echo Reply. Cela permet de vérifier la connectivité et le délai de réponse.

### Exemples de commandes

**Windows:**
```cmd
ping 8.8.8.8
ping -t 8.8.8.8          # Ping continu jusqu'à Ctrl+C
ping -n 10 192.168.1.1   # Envoie 10 paquets
```

**Linux:**
```bash
ping 8.8.8.8
ping -c 4 8.8.8.8        # Envoie 4 paquets puis arrête
ping -i 2 8.8.8.8        # Intervalle de 2 secondes entre les paquets
```

## traceroute

`traceroute` utilise ICMP (ou UDP/TCP) pour identifier les routeurs intermédiaires sur le chemin vers une destination.

La commande envoie des paquets avec TTL progressivement croissants. Chaque routeur qui reçoit un paquet avec TTL=1 le décrémente à 0 et envoie un message ICMP Time Exceeded, révélant ainsi son adresse IP.

### Exemples de commandes

**Windows (tracert):**
```cmd
tracert 8.8.8.8
tracert -h 30 8.8.8.8    # Nombre maximum de sauts
```

**Linux (traceroute):**
```bash
traceroute 8.8.8.8
traceroute -m 30 8.8.8.8 # Nombre maximum de sauts
traceroute -I 8.8.8.8    # Utilise ICMP au lieu d'UDP
```

