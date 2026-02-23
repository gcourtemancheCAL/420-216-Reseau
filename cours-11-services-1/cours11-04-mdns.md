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
- Simple à déployer et à utiliser

**Faiblesses:**
- Limité aux réseaux locaux (ne traverse pas les routeurs par défaut)
- Génère du trafic broadcast sur le réseau
- Peut créer des conflits de noms si mal géré
- Pas de sécurité intégrée (DNSSEC non supporté)
- Performance limitée sur les grands réseaux
