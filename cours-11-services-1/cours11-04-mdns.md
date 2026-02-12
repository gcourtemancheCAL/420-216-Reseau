# mDNS

**Sommaire**

mDNS (Multicast DNS) est un protocole de résolution de noms permettant aux appareils de découvrir et de résoudre des noms d'hôtes en adresses IP sur un réseau local sans serveur DNS centralisé. Il fait partie de la technologie Zeroconf (Zero Configuration Networking) et est utilisé par des services comme Bonjour (Apple), Avahi (Linux) et diverses implémentations Windows. mDNS utilise le domaine spécial `.local` et la multidiffusion pour diffuser les requêtes.

**Protocole de transport:** UDP

**Port:** 5353

## Forces et faiblesses

**Forces:**
- Configuration automatique sans infrastructure DNS
- Fonctionne sans serveur centralisé (peer-to-peer)
- Idéal pour les petits réseaux et réseaux domestiques
- Découverte automatique de services (avec DNS-SD)
- Simple à déployer et à utiliser
- Réduit la charge administrative
- Supporte IPv4 et IPv6
- Permet la résolution de noms conviviaux (nom.local)

**Faiblesses:**
- Limité aux réseaux locaux (ne traverse pas les routeurs par défaut)
- Génère du trafic de multidiffusion sur le réseau
- Peut créer des conflits de noms si mal géré
- Pas de sécurité intégrée (DNSSEC non supporté)
- Performance limitée sur les grands réseaux
- Incompatible avec certains équipements réseau anciens
- Le domaine `.local` peut entrer en conflit avec Active Directory

## Cas d'utilisations

- **Découverte d'imprimantes** sur le réseau local
- **Partage de fichiers** : découverte automatique de serveurs de fichiers
- **Streaming média** : découverte d'appareils Apple TV, Chromecast, etc.
- **Services IoT** : découverte d'appareils connectés (caméras, thermostats, etc.)
- **Développement web** : accès à des serveurs locaux par nom convivial
- **Jeux en réseau local**
- **Communication entre applications** sur le même réseau
- **Synchronisation automatique** entre appareils (ex: iTunes)
- **Configuration réseau domestique** sans DNS
- **Services Apple (AirPrint, AirPlay, etc.)**
