# Atelier: Installation et configuration du service SSH/SFTP

## Objectifs
- Installer et configurer le service SSH (openssh-server) sur Linux
- Modifier les paramètres de configuration (port et adresse d'écoute)
- Tester la connexion SSH depuis Windows avec PuTTY
- Tester le transfert de fichier via SFTP avec WinSCP
- Valider avec la commande `ss`
- Gérer le service avec systemctl
- Désactiver la connexion root et tester l'authentification

---

## Prérequis
- Une machine virtuelle Linux (Ubuntu/Debian)
- Une machine Windows avec accès au réseau
- PuTTY (pour SSH) - [Lien de téléchargement](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- WinSCP (pour SFTP) - [Lien de téléchargement](https://winscp.net/eng/download.php)
- Certaines commandes vont nécessiter les accès privilégiés. Connectez-vous avec root ou assurez-vous d'avoir accès à sudo.

---

## Exercice 1: Installation et configuration du service SSH

### Partie A: Installer le serveur SSH

1. **Sur la machine Linux, installer le serveur SSH:**
   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```

2. **Vérifier que le service est actif:**
   ```bash
   sudo systemctl status ssh
   ```

3. **Activez le service au démarrage (s'il ne l'est pas déjà):**
   ```bash
   sudo systemctl enable ssh
   ```

4. **Utilisez la commande `ss` pour identifier si le service SSH est actif:**
   
   SSH est un protocole applicatif utilisant *tcp* comme transport, et qui *écoute* sur le *port 22* par défaut.
   
   ```bash
   ss -tlnp | grep ssh
   ```
   
   Vous devriez voir une ligne similaire à:
   ```
   LISTEN    0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=XXXX,fd=3))
   ```

### Partie B: Test de connexion depuis Windows

5. **Sur Windows, lancez PuTTY et configurez la connexion:**
   - Host Name (or IP address): Adresse IP de votre serveur Linux
   - Port: 22
   - Connection type: SSH
   - Cliquez sur "Open"

6. **Se connecter à votre serveur Linux:**
   - Acceptez la clé d'hôte si c'est la première connexion
   - Saisissez votre nom d'utilisateur Linux
   - Saisissez votre mot de passe

7. **Une fois connecté via SSH:**
   - Affichez tous les sockets TCP sur votre système Linux
     ```bash
     ss -tn
     ```
   - Vous devriez voir deux connection SSH (l'une en LISTEN, l'autre en ESTAB)
   - Pouvez-vous identifier votre système Windows dans ces sockets?

---

## Exercice 2: Test de transfert de fichier via SFTP

### Partie A: Transférer un fichier avec SFTP

8. **Sur Windows, lancez WinSCP et configurez la connexion:**
   - Host name: Adresse IP de votre serveur Linux
   - Port: 22
   - User name: Votre nom d'utilisateur Linux
   - Password: Votre mot de passe Linux
   - File protocol: SFTP

9. **Effectuez un transfert de fichier:**
   - Transférez un fichier de votre système Windows vers Linux
   - Validez que le fichier a bien été transféré sur le serveur Linux
   
   ```bash
   ls -la ~
   ```

10. **Transférez un fichier depuis le serveur vers Windows:**
    - Créez un fichier de test sur Linux
      ```bash
      echo "Contenu du fichier de test" > ~/fichier-test.txt
      ```
    - Téléchargez-le via SFTP dans WinSCP

---

## Exercice 3: Modification de la configuration SSH

### Partie A: Modifier le port d'écoute

11. **Modifier le fichier de configuration SSH:**
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Cherchez et modifiez la ligne `#Port 22` (elle est commentée) en:
    ```
    Port 2222
    ```

12. **Redémarrez le service SSH:**
    ```bash
    sudo systemctl restart ssh
    ```

13. **Validez le changement avec `ss`:**
    ```bash
    ss -tlnp | grep ssh
    ```
    
    Le service devrait maintenant écouter sur le port 2222.

14. **Testez la connexion avec le nouveau port:**
    - Lancez PuTTY
    - Host Name: Adresse IP de votre serveur
    - Port: 2222
    - Connection type: SSH
    - Connectez-vous

### Partie B: Modifier l'adresse d'écoute

15. **Modifier le fichier de configuration SSH à nouveau:**
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Cherchez et modifiez la ligne ou ajoutez `#Port 2222` en:
    ```
    #Port 2222
    Port 22
    ListenAddress 127.0.0.1
    ```

16. **Redémarrez le service SSH:**
    ```bash
    sudo systemctl restart ssh
    ```

17. **Validez avec `ss`:**
    ```bash
    ss -tlnp | grep ssh
    ```
    
    Le service devrait maintenant écouter uniquement sur 127.0.0.1 (localhost).

18. **Essayez de vous connecter depuis Windows au port 22:**
    - Est-ce que ça fonctionne? Qu'est-ce qui vous en empêche?
    - Pourquoi?

19. **Modifier l'adresse d'écoute à votre adresse IP:**
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Remplacez `ListenAddress 127.0.0.1` par `ListenAddress 192.168.x.x` (votre adresse IP réelle)

20. **Redémarrez et validez:**
    ```bash
    sudo systemctl restart ssh
    ss -tlnp | grep ssh
    ```

21. **Testez la connexion depuis Windows:**
    - Lancez PuTTY avec votre adresse IP
    - Port: 22
    - Est-ce que ça fonctionne?

22. **Rétablir la configuration par défaut:**
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Modifiez en:
    ```
    Port 22
    #ListenAddress 127.0.0.1
    ```
    
    ```bash
    sudo systemctl restart ssh
    ```

---

## Exercice 4: Gestion avancée avec systemctl et journalctl

23. **Afficher l'état détaillé du service SSH:**
    ```bash
    systemctl status ssh
    ```

24. **Consulter les journaux SSH:**
    ```bash
    # Voir les 20 derniers messages
    journalctl -u ssh -n 20
    
    # Suivre les logs en temps réel
    journalctl -u ssh -f
    
    # Voir les erreurs uniquement
    journalctl -u ssh -p err
    ```

25. **Faire une tentative de connexion échouée et observer les logs:**
    - Depuis Windows, essayez une connexion SSH avec un mauvais mot de passe
    - Observez les logs sur Linux:
      ```bash
      journalctl -u ssh -f
      ```

---

## Exercice 5: Sécurité - Désactiver la connexion root

### Partie A: Désactiver la connexion SSH pour root

26. **Modifier le fichier de configuration SSH:**
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Cherchez la ligne `#PermitRootLogin yes` et remplacez-la par:
    ```
    PermitRootLogin no
    ```

27. **Redémarrez le service SSH:**
    ```bash
    sudo systemctl restart ssh
    ```

28. **Testez la restriction:**
    - Depuis Windows, essayez de vous connecter en tant que `root` avec SSH
    - Vous devriez obtenir un message d'erreur de connexion refusée

29. **Confirmez que vous pouvez toujours vous connecter avec un utilisateur normal:**
    - Connectez-vous avec votre utilisateur habituel (pas root)
    - Vous devriez pouvoir utiliser `sudo` pour les opérations administratives

---

## Questions de révision

1. Quels sont les avantages de SSH par rapport à Telnet?
2. Comment pouvez-vous identifier quel service écoute sur quel port avec `ss`?
3. Pourquoi est-il important de pouvoir modifier le port d'écoute d'un service?
4. Qu'est-ce que ListenAddress 127.0.0.1 signifie et pourquoi serait-ce utile?
5. Pourquoi est-il recommandé de désactiver la connexion root via SSH?
6. Quelle est la différence entre `sudo systemctl restart` et `sudo systemctl reload`?
7. Comment SFTP améliore-t-il FTP en termes de sécurité?

