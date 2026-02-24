# Atelier: Installation et configuration du service HTTP/HTTPS (Apache2)

## Objectifs
- Installer et configurer le service Apache2 (serveur web) sur Linux
- Configurer un site web statique
- Tester l'accès au serveur web depuis Windows via HTTP
- Valider avec la commande `ss`
- Gérer le service avec systemctl
- Configurer HTTPS avec des certificats auto-signés
- Rendre le site accessible via un domaine .local avec mDNS

---

## Prérequis
- Une machine virtuelle Linux (Ubuntu/Debian) avec adaptateur en mode "Bridged"
- Configuration IP statique sur la VM Linux
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
   
   ```bash
   ss -tlnp | grep apache2
   ```
   
   Vous devriez voir une ligne similaire à:
   ```
   LISTEN    0      512          0.0.0.0:80        0.0.0.0:*    users:(("apache2",pid=XXXX,fd=3))
   ```

5. **Identifiez le répertoire racine du serveur web:**
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
   - Sur Linux, affichez les sockets TCP actifs:
     ```bash
     ss -tn
     ```
   - Vous devriez voir une connexion en état ESTAB correspondant à votre navigateur Windows

---

## Exercice 2: Gérer les permissions et créer une page web personnalisée

### Partie A: Gérer les permissions du répertoire www

8. **Vérifier le propriétaire et les permissions du répertoire:**
   ```bash
   ls -ld /var/www/html/
   ```

9. **Créer une page HTML personnalisée:**
   ```bash
   # Option 1: Utiliser nano pour créer/éditer la page
   sudo nano /var/www/html/index.html
   ```
   
   Remplacez le contenu par une page HTML personnalisée (exemple):
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Mon serveur web personnel</title>
       <style>
           body { font-family: Arial, sans-serif; margin: 50px; }
           h1 { color: #333; }
       </style>
   </head>
   <body>
       <h1>Bienvenue sur mon serveur web!</h1>
       <p>Ceci est une page de test hébergée sur Apache2.</p>
       <p>IP du serveur: 192.168.x.x</p>
   </body>
   </html>
   ```

10. **Vérifier que la page est bien accessible depuis Windows:**
    - Rafraîchissez votre navigateur (F5 ou Ctrl+R)
    - Vous devriez voir votre page personnalisée

### Partie B: Transférer une page web via SFTP

11. **Créer une page HTML plus avancée sur Windows:**
    - Créez un fichier `page-avancee.html` sur votre ordinateur Windows avec du contenu personnalisé

12. **Transférez la page via SFTP (WinSCP):**
    - Ouvrez WinSCP
    - Connectez-vous à votre serveur Linux (voir Exercice 1 du cours 12 pour les détails)
    - Naviguez vers `/var/www/html/`
    - Faites glisser-déposer votre fichier HTML vers le serveur

13. **Accédez à la nouvelle page depuis Windows:**
    - Dans votre navigateur, allez à: `http://192.168.1.100/page-avancee.html`
    - Vérifiez que la page s'affiche correctement

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
    - Dans votre navigateur Windows, allez à: `http://192.168.1.100:8080`
    - Vérifiez que la page s'affiche toujours

### Partie B: Rétablir le port par défaut et vérifier la configuration

18. **Revenir au port 80:**
    ```bash
    sudo nano /etc/apache2/ports.conf
    ```
    
    Remplacez `Listen 8080` par:
    ```
    Listen 80
    ```

19. **Redémarrez le service:**
    ```bash
    sudo systemctl restart apache2
    ss -tlnp | grep apache2
    ```

---

## Exercice 4: Configuration HTTPS avec certificats auto-signés

### Partie A: Installer les outils SSL

20. **Installer OpenSSL (généralement déjà présent):**
    ```bash
    sudo apt install openssl
    ```

21. **Activer le module SSL d'Apache2:**
    ```bash
    sudo a2enmod ssl
    ```

22. **Vérifier que le module est activé:**
    ```bash
    sudo apache2ctl -M | grep ssl
    ```

### Partie B: Générer un certificat auto-signé

23. **Créer un répertoire pour les certificats:**
    ```bash
    sudo mkdir -p /etc/apache2/ssl
    ```

24. **Générer une clé privée et un certificat auto-signé valid 365 jours:**
    ```bash
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/apache2/ssl/private.key \
    -out /etc/apache2/ssl/certificate.crt
    ```
    
    Vous serez invité à entrer des informations (pays, état, ville, etc.). Vous pouvez appuyer sur Entrée pour les laisser vides sauf pour "Common Name" où vous devez entrer votre nom de domaine ou votre adresse IP.

25. **Vérifier les fichiers créés:**
    ```bash
    ls -la /etc/apache2/ssl/
    ```

### Partie C: Configurer Apache2 pour HTTPS

26. **Créer un fichier de configuration SSL pour votre site:**
    ```bash
    sudo nano /etc/apache2/sites-available/default-ssl.conf
    ```
    
    Vérifiez ou ajoutez les lignes suivantes:
    ```apache
    <VirtualHost *:443>
        ServerName votre-domaine-local
        DocumentRoot /var/www/html
        
        SSLEngine on
        SSLCertificateFile /etc/apache2/ssl/certificate.crt
        SSLCertificateKeyFile /etc/apache2/ssl/private.key
    </VirtualHost>
    ```

27. **Activer le site SSL:**
    ```bash
    sudo a2ensite default-ssl.conf
    ```

28. **Tester la configuration:**
    ```bash
    sudo apache2ctl configtest
    ```
    
    Vous devriez obtenir "Syntax OK".

29. **Redémarrez Apache2:**
    ```bash
    sudo systemctl restart apache2
    ```

30. **Vérifiez que le port 443 est actif:**
    ```bash
    ss -tlnp | grep apache2
    ```
    
    Vous devriez voir deux lignes, une pour le port 80 (HTTP) et une pour le port 443 (HTTPS).

31. **Testez HTTPS depuis Windows:**
    - Dans votre navigateur, allez à: `https://192.168.1.100`
    - Vous verrez probablement un avertissement de certificat (c'est normal pour un auto-signé)
    - Cliquez sur "Accepter le risque et continuer" ou équivalent
    - Vérifiez que votre page s'affiche

---

## Exercice 5: Configurer mDNS pour accès par nom de domaine

### Partie A: Installer et configurer Avahi (si pas déjà fait)

32. **Installer Avahi:**
    ```bash
    sudo apt install avahi-daemon avahi-utils
    ```

33. **Vérifier que le service est actif:**
    ```bash
    sudo systemctl status avahi-daemon
    ```

34. **Vérifier ou définir le hostname:**
    ```bash
    hostname
    # Pour modifier:
    sudo hostnamectl set-hostname mon-serveur-web
    sudo systemctl restart avahi-daemon
    ```

### Partie B: Accéder au site via domaine .local

35. **Depuis Windows, accédez au site via le domaine .local:**
    - HTTP: `http://mon-serveur-web.local`
    - HTTPS: `https://mon-serveur-web.local`
    - Les pages devraient s'afficher normalement
    - Pour HTTPS, acceptez l'avertissement du certificat auto-signé

36. **Vérifiez la résolution de noms:**
    ```powershell
    # Depuis Windows PowerShell
    ping mon-serveur-web.local
    nslookup mon-serveur-web.local
    ```

---

## Exercice 6: Gestion avancée avec systemctl et journalctl

37. **Afficher l'état détaillé du service Apache2:**
    ```bash
    systemctl status apache2
    ```

38. **Consulter les journaux Apache2:**
    ```bash
    # Voir les 20 derniers messages
    journalctl -u apache2 -n 20
    
    # Suivre les logs en temps réel
    journalctl -u apache2 -f
    
    # Voir les erreurs uniquement
    journalctl -u apache2 -p err
    
    # Voir les logs d'accès Apache
    sudo tail -f /var/log/apache2/access.log
    
    # Voir les logs d'erreur Apache
    sudo tail -f /var/log/apache2/error.log
    ```

39. **Accéder à votre site depuis Windows en observant les logs:**
    - Ouvrez le log en temps réel sur Linux: `sudo tail -f /var/log/apache2/access.log`
    - Accédez à votre site depuis Windows
    - Observez les entrées dans le log (adresse IP, navigateur, etc.)

---

## Questions de révision

1. Quel est le port par défaut d'un serveur web HTTP?
2. Quel est le port par défaut d'un serveur web HTTPS?
3. Où se trouve le répertoire racine des fichiers web pour Apache2?
4. Pourquoi utiliser HTTPS plutôt que HTTP?
5. Qu'est-ce qu'un certificat auto-signé et pourquoi génère-t-il un avertissement?
6. Quelle est la différence entre mDNS et DNS standard?
7. Comment modifier le port d'écoute d'Apache2?
8. Où sont stockés les certificats SSL dans Apache2?
9. Comment tester la syntaxe de configuration d'Apache2?
10. Pourquoi est-il utile d'utiliser mDNS pour accéder au serveur par un nom plutôt que par une adresse IP?