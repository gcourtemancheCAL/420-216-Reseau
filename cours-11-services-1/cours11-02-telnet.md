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
- Vulnérable aux attaques man-in-the-middle
- Non recommandé pour la majorité des situations.

Telnet est principalement utilisé pour permettre un accès point à point avec certains équipements particulier. Dans ce contexte, la simplicité du protocole est un avantage prépondérant, et la sécurité de l'échange n'est plus considérable.

**Note:** Pour les connexions sécurisées, SSH (Secure Shell) a largement remplacé Telnet dans les environnements modernes.
