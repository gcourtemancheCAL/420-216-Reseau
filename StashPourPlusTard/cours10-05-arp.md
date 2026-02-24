# ARP

## IMPORTANT : On va couvrir ça après l'intra

## Description et rôle de ARP

ARP (Address Resolution Protocol) est un protocole de la couche 2/3 qui résout les adresses IP en adresses MAC dans un domaine de diffusion local.

Sur Ethernet, lorsqu'un hôte doit envoyer un paquet à une adresse IP locale, il doit l'identifier par son adresse MAC. ARP lui permet de découvrir cette adresse.

### Fonctionnement

1. L'hôte envoie une demande ARP (ARP Request) en broadcast: "Qui a l'adresse IP 192.168.1.10?"
2. L'hôte possédant cette IP répond avec une réponse ARP (ARP Reply) contenant son adresse MAC
3. L'hôte demandeur met en cache la correspondance IP-MAC
4. Pendant quelques minutes, les communications suivantes n'ont pas besoin d'une nouvelle demande ARP

## Cache ARP

Le cache ARP stocke les associations IP-MAC découvertes récemment. Cela évite de faire une demande ARP à chaque paquet.

### Caractéristiques

- **Durée de vie:** Typiquement 15 à 240 secondes selon le système
- **Statique vs Dynamique:** Les entrées peuvent être ajoutées manuellement (statique) ou découvertes automatiquement (dynamique)
- **Expiration:** Les entrées anciennes sont supprimées du cache

## Commandes Windows

### Afficher le cache ARP
```cmd
arp -a                    # Affiche tout le cache ARP
arp -a -N 192.168.1.1    # Cache ARP pour une interface spécifique
```

### Ajouter une entrée statique
```cmd
arp -s 192.168.1.10 00:11:22:33:44:55
```

### Supprimer une entrée
```cmd
arp -d 192.168.1.10       # Supprime une entrée spécifique
arp -d *                  # Vide tout le cache ARP
```

### Exemple de sortie
```
Interface: 192.168.1.100 --- 0x4
  Internet Address      Physical Address      Type
  192.168.1.1           a4-ba-db-12-34-56     dynamic
  192.168.1.50          00-11-22-33-44-55     static
```

## Commandes Linux

### Afficher le cache ARP
```bash
ip neigh show             # Affiche les voisins connus
```
