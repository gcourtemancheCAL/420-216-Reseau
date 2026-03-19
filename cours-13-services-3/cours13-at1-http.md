# Atelier: Installation et configuration du service HTTP (Apache2)

## Objectifs
- Installer et configurer le service Apache2 (serveur web) sur Linux
- Configurer un site web statique
- Tester l'accès au serveur web depuis Windows via HTTP
- Valider avec la commande `ss`
- Gérer le service avec systemctl
- Rendre le site accessible via un domaine .local avec mDNS

---

## Prérequis
- Une machine virtuelle Linux (Ubuntu/Debian) avec adaptateur en mode "Bridged"
- Une machine Windows avec accès au réseau
- Navigateur web (Firefox, Chrome, Edge, etc.)
- Accès sudo ou root sur la machine Linux

---

## Exercice 1: Installation et configuration du service Apache2

### Partie A: Installer et démarrer le serveur Apache2

1. **Sur la machine Linux, installer Apache2:**
   ```bash
   sudo apt update
   sudo apt install apache2
   ```

2. **Vérifier que le service est actif:**
   ```bash
   sudo systemctl status apache2
   ```

3. **Activez le service au démarrage (s'il ne l'est pas déjà):**
   ```bash
   sudo systemctl enable apache2
   ```

4. **Utilisez la commande `ss` pour identifier si le service Apache2 est actif:**
   
   Apache2 est un protocole applicatif utilisant *tcp* comme transport, et qui *écoute* sur le *port 80* par défaut (HTTP).
   
4. **Identifiez le répertoire racine du serveur web:**
   ```bash
   ls -la /var/www/html/
   ```
   
   Vous devriez voir un fichier `index.html` par défaut.

### Partie B: Test de connexion depuis Windows

6. **Sur Windows, ouvrez votre navigateur web et accédez à votre serveur:**
   - Tapez l'adresse IP de votre serveur Linux dans la barre d'adresse
   - Exemple: `http://192.168.1.100`
   - Vous devriez voir la page d'accueil par défaut d'Apache2

7. **Vérifiez les connexions TCP:**
   - Sur Linux, affichez les sockets TCP actifs en boucle:
     ```bash
     for i in `seq 2000` ; do ss -tn ; done
     # ctrl + c pour intérrompre lorsque le 
     # socket apparait. 
     
     # Alternative : 
     for i in $(seq 2000) ; do ss -tn ; done
     ```
   - Rafraichissez la page sur Windows.
   - Vous devriez voir une connexion en état ESTAB éventuellement apparaitre correspondant à votre navigateur Windows

---

## Exercice 2: Créer une page web personnalisée

### Partie A: Gérer les permissions du répertoire www

8. **Vérifier le propriétaire et les permissions du répertoire:**
   ```bash
   ls -ld /var/www/html/
   ```

9. **Créer une page HTML personnalisée:**
Utilisez ChatGPT pour créer la page html de vos rêves. Il est impératif qu'elle soit parfaite en tout point.

À l'aide de sftp, transférez cette page sur la machine linux. 
- Vous allez avoir besoin d'installer sshd.
- Vous allez probablement avoir à faire le transfert en 2 étapes
	- Transférer les fichiers de la page sur linux avec votre compte utilisateur
	- Déplacez les fichiers de la page dans `/var/www/html` avec le compte root
- Les fichiers de la page web doivent être disponible en lecture par le service apache2 qui opère normalement sous l'utilisateur `www-data` et le groupe `www-data`. Assurez-vous donc que vos fichiers sont lisibles par les utilisateurs non propriétaires (`others`) 
-
10. **Vérifier que la page est bien accessible depuis Windows:**
    - Si la page se nomme index.html, vous n'avez qu'à vous connecter directement à votre serveur.
    - Sinon, vous aurez besoin d'entrer son nom dans l'URL.
    - Le chemin de l'url va être converti en chemin relatif à partir du dossier `/var/www/html`

---

## Exercice 3: Modification de la configuration Apache2

### Partie A: Modifier le port d'écoute

14. **Modifier le fichier de configuration Apache2:**
    ```bash
    sudo nano /etc/apache2/ports.conf
    ```
    
    Cherchez et modifiez la ligne `Listen 80` en:
    ```
    Listen 8080
    ```

15. **Redémarrez le service Apache2:**
    ```bash
    sudo systemctl restart apache2
    ```

16. **Validez le changement avec `ss`:**
    ```bash
    ss -tlnp | grep apache2
    ```
    
    Le service devrait maintenant écouter sur le port 8080.

17. **Testez la connexion avec le nouveau port:**
    - Dans votre navigateur Windows, connectez-vous à votre serveur sur le port 8080
    - Vérifiez que la page s'affiche toujours

18. **Configurez l'adresse d'écoute:**
    - Modifiez la configuration en remplaçant la ligne `Listen` par les deux lignes suivantes : 
```
	Listen 127.0.0.1:8080
	Listen <votre adresse IPv4>:80
```

Redémarrez le service.

Observez les sockets TCP à l'aide de `ss`.

Testez la connexion à l'aide de Windows. Ça devrait fonctionner normalement.

Sur Linux, effectuez les tests suivants : 
- Envoyez une requête http vers le 127.0.0.1:80
- Envoyez une requête http vers le 127.0.0.1:8080
- Envoyez une requête http vers votre adresse IP

Qu'est-ce que vous observé?

### Partie B: Rétablir le port par défaut et vérifier la configuration

18. **Revenir au port 80:**
    ```bash
    sudo nano /etc/apache2/ports.conf
    ```
    
    Remplacez les lignes `Listen ...` par:
    ```
    Listen 80
    ```

19. **Redémarrez le service:**
    ```bash
    sudo systemctl restart apache2
    ss -tlnp | grep apache2
    ```

---

## Exercice 4: Configurer mDNS pour accès par nom de domaine

### Partie A: Installer et configurer Avahi (si pas déjà fait)

32. Installez Avahi
33. Configurez votre système de sorte à ce qu'il puisse être accédé via un nom d'hôte vous étant unique.
	1. Exemple : gcourtemanche11.local
34. Testez la connexion.

---

## Exercice 5 - Configurer apache pour https

**NB. Bien que cet exercice soit plus authentique dans la mesure que la plupart des serveurs web vont fonctionner via https, il en demeure facultatif dans le cadre de ce cours. Je vous recommende néanmoins de l'essayer si vous en avez le temps**

[Des instructions se trouvent ici](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-18-04) pour configurer le serveur apache de sorte à ce que vous puissiez servir le contenu via https.

https est préféré à http puisque l'échange de données est chiffré et donc sécurisé.

https est cependant plus complexe que http puisqu'il nécessite l'utilisation de certificats afin de valider l'authenticité du serveur et de permettre le chiffrement de la connexion. Normalement, ces certificats sont livrés par une authorité fiable en échange d'argent. Nous allons ici généré un certificat nous-même. Cette pratique est moins sécuritaire, mais néanmoins plus que de fonctionner simplement avec http. 