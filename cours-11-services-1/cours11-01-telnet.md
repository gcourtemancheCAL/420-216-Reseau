# telnet

## Sommaire
**Sommaire**

Telnet (Telecommunication Network) est un protocole d'application permettant l'émulation de terminal et la communication en mode texte avec un hôte distant. Il permet à un utilisateur de se connecter à distance à un serveur et d'exécuter des commandes comme s'il était physiquement présent à la console. Telnet fonctionne selon un modèle client-serveur où le client envoie des commandes et le serveur retourne les résultats.

**Protocole de transport:** TCP

**Port:** 23

## Forces et faiblesses

**Forces:**
- Simple à utiliser et à implémenter
- Compatible avec la plupart des systèmes d'exploitation
- Utile pour tester la connectivité et déboguer des services réseau
- Léger en termes de ressources système
- Permet l'accès à distance pour l'administration système

**Faiblesses:**
- Aucun chiffrement : toutes les données, y compris les mots de passe, sont transmises en texte clair
- Vulnérable aux attaques par interception (man-in-the-middle)
- Pas d'authentification forte
- Obsolète pour les connexions sécurisées
- Non recommandé pour les réseaux publics ou non sécurisés

## Cas d'utilisations

- **Administration de serveurs** (environnements isolés ou réseaux privés sécurisés uniquement)
- **Test de connectivité** vers des services réseau (HTTP, SMTP, etc.)
- **Débogage** de protocoles en mode texte
- **Accès à des équipements réseau** anciens ne supportant pas SSH
- **Accès à des BBS (Bulletin Board Systems)** ou systèmes hérités
- **Environnement d'apprentissage** pour comprendre les protocoles réseau

**Note:** Pour les connexions sécurisées, SSH (Secure Shell) a largement remplacé Telnet dans les environnements modernes.
