# Atelier: Installation et configuration du service SSH/SFTP

## Objectifs
- Installer et configurer le service SSH (openssh-server) sur Linux
- Modifier les paramètres de configuration (port et adresse d'écoute)
- Tester la connexion SSH depuis Windows avec PuTTY
- Tester le transfert de fichier via SFTP avec WinSCP
- Valider avec la commande `ss`
- Gérer le service avec systemctl
- Désactiver la connexion root et tester l'authentification
- Tester la configuration d'adresse d'écoute

---

## Prérequis
- Un système Linux (Ubuntu/Debian)
- Une machine Windows avec accès au réseau
- PuTTY (pour SSH) - [Lien de téléchargement](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- WinSCP (pour SFTP) - [Lien de téléchargement](https://winscp.net/eng/download.php)
- Certaines commandes vont nécessiter les accès privilégiés. Connectez-vous avec root ou assurez-vous d'avoir accès à sudo.

### En classe

En classe l'atelier va être réalisé sur une machine physique dans laquelle une seconde carte réseau sera installé.

Les paramètres de configurations IP vous seront donner.

### À la maison

Une seconde carte réseau peut être ajoutée à la machine virtuelle. Vous pouvez paramétrer vos interfaces via DHCP.

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

### Partie B: Test de connexion depuis Windows

5. **Sur Windows, lancez PuTTY et configurez la connexion:**
   - Host Name (or IP address): Adresse IP de votre serveur Linux
   - Port: 22
   - Connection type: SSH
   - Cliquez sur "Open"

<img src="img/Pasted image 20260225090142.png" width="800" />

5. **Se connecter à votre serveur Linux:**
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

10. **Transférez un fichier depuis le serveur vers Windows:**
    - Créez un fichier de test sur Linux. Ajoutez y le contenu de vos rêves.
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

15. **Testez la connexion l'ancien port:**
    - Lancez PuTTY
    - Host Name: Adresse IP de votre serveur
    - Port: 22
    - Connection type: SSH


### Partie B: Modifier l'adresse d'écoute

15. **Modifier le fichier de configuration SSH à nouveau:**
 ```bash
 sudo nano /etc/ssh/sshd_config
 ```

Changez la configuration de port et remettez la à sa valeur par défaut.

Changez l'adresse d'écoute pour le 127.0.0.1.

```
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

17. **Essayez de vous connecter à vous-même à partir du système linux.

```bash
    ssh nomutilisateur@127.0.0.1
```

**Question:** Est-ce que ça fonctionne?

Et si vous utilisez l'adresse IP de l'une de vos interface ethernet?

**Pourquoi?**

18. **Essayez de vous connecter depuis Windows au port 22:**
    - Testez avec l'adresse de loopback. Est-ce que ça fonctionne? Pourquoi?
    - Testez avec l'adresse des deux interfaces ethernet. Est-ce que ça fonctionne? Pourquoi?

19. **Modifier l'adresse d'écoute pour l'adresse ip de l'une de vos interface ethernet:**
	- N'oubliez pas de redémarrer le service!
	- Testez la connexion en local sur l'adresse de loopback ainsi que les adresses de vos 2 interfaces. Que remarquez-vous?
	- Essayez de vous connecter à partir de Windows en utilisant l'adresse 

20. **Redémarrez et validez:**
    ```bash
    sudo systemctl restart ssh
    ss -tlnp | grep ssh
    ```

21. **Testez la connexion depuis Windows:**
    - Testez avec les deux interfaces. Seulement l'une d'entre elle devrait fonctionner.

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

### Partie A: Connexion SSH pour root

26. **Essayez de vous connecter via ssh avec l'utilisateur root:**
	1. Le test peut être fait avec putty ou sur linux directement.
	2. Vous ne devriez pas être en mesure de le faire.

27. **Modifier le fichier de configuration SSH:**
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
    Cherchez la ligne `#PermitRootLogin` et remplacez-la par:
    ```
    PermitRootLogin yes
    ```

28. **Redémarrez le service SSH:**
    ```bash
    sudo systemctl restart ssh
    ```

29. **Testez de nouveau:**
    - Depuis Windows, essayez de vous connecter en tant que `root` avec SSH
    - Ça devrait fonctionner cette fois-ci.

Par mesure de sécurité, il est normalement fortement recommandé de désactiver la connexion root via ssh.

Modifier donc la configuration pour le désactiver.

---