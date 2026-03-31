# Exercice de configuration d'un réseau simple sur Packet Tracer

## Consignes

À partir du fichier `cours17-ex1.pkt`, complétez la topologie suivante : 

<img src="img/Pasted image 20260323113249.png" width="600" />

### Paramètres :

**RLAN :**
- L'adresse de son interface le connectant à internet doit être le 172.16.0.2 avec un masque sur 24 bits.
- Son interface dans le LAN doit être le 192.168.0.1 avec un masque sur 24 bits.
- R1 doit avoir une route par défaut vers le 172.16.0.1.
- Assurez-vous de sauvegarder sa configuration

**SW2 :** 
- Désactiver les interfaces de SW2 qui ne sont pas utilisées.
- Assurez-vous de sauvegarder votre configuration.

Vous pouvez configurer plusieurs interfaces en même temps avec la commnande interface en utilisant un range. Les 3 exemples suivants vont configurer les interfaces FastEthernet de 1 à 24 en même temps : 

```
interface range FastEthernet 0/1 - FastEthernet 0/24
interface range FastEthernet 0/1 - 24
interface range fa0/1-24
```

**PC0, PC1, PC2, PC3 :** 
- Attribuez une configuration statique à chacun de ces PCs.
- N'assignez pas de serveur DNS
- La passerelle va être, bien entendu, RLAN.

### Validation :
- Tous les PCs devraient pouvoir se rejoindre mutuellement  à l'aide de ping.
- Tous les PCs devraient pouvoir rejoindre le 8.8.8.8 à l'aide de ping.
- RLAN devrait pouvoir ping le 8.8.8.8.
- Les équipements de votre lan (routeur et commutateurs) devraient conserver leur configuration après un redémarrage.
