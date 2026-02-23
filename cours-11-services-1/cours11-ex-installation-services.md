# Atelier: Installation et configuration de services Telnet, FTP et mDNS

## Objectifs
- Installer et configurer les services Telnet et FTP sur Linux
- Modifier les paramètres de configuration (adresse d'écoute et port)
- Tester la connectivité depuis Windows avec Putty (Telnet) et WinSCP (FTP)
- Installer et configurer Avahi pour fournir un nom de domaine local via mDNS
- Valider la résolution de noms avec mDNS
- Comprendre les limitations de nslookup avec mDNS

---

## Prérequis
- Une machine virtuelle Linux
- Une machine Windows avec accès au réseau
- Putty (pour Telnet) - [Lien de téléchargement](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- WinSCP (pour FTP) - [Lien de téléchargement](https://winscp.net/eng/download.php)
- Certaines commandes vont necéssiter les accès privilégiés. Connectez-vous avez root ou assurez-vous d'avoir accès à sudo.

---

## Exercice 1: Installation et configuration de Telnet

### Partie A: Installer le serveur Telnet

1. **Sur la machine Linux, installer le serveur Telnet:**
   ```bash
   sudo apt update
   sudo apt install telnetd
   ```

2. **Utilisez la commande `ss` pour identifier si le service `telnetd` est actif**

telnet est un protocole applicatif utilisant *tcp* comme transport, et qui *écoute* sur le *port 23* par défaut.

3. **Activation de telnet**

`telnetd` va être contrôler par le service `inetutils-inetd`.  Par défaut, le service `telnet` est désactiver puisque le protocole n'est pas sécuritaire (la communication n'est pas chiffré).

Nous allons modifier la configuration de `inetutils-inetd` afin d'activer telnet.

Modifiez le fichier `/etc/inetd.conf` afin d'y ajouter la ligne suivante : 

```bash
telnet stream tcp nowait root /usr/sbin/tcpd /usr/sbin/telnetd
```

Redémarrez le service `inetutils-inetd` 

```bash
systemctl restart inetutils-inetd
```

4. **Utilisez la commande `ss` pour identifier si le service `telnetd` est actif**

Vous devriez avoir un socket actif pour telnet.

5. **Test de connexion**

Sur Windows, utilisez putty pour établir une connexion telnet vers votre système linux. 

À partir de putty, validez que vous êtes bien connecté à votre VM linux (vous pouvez regarder l'adresse IP, le nom d'hôte, les utilisateurs...)

À partir de votre session telnet, affichez tous les sockets TCP sur votre système Linux. Vous devriez en voir deux pour telnetd (l'un en LISTEN, l'autre en ESTAB). 

Pouvez-vous identifier votre système Windows dans ces sockets?

---

## Exercice 2: Installation et configuration de FTP

### Partie A: Installer le serveur FTP

6. **Sur la machine Linux, installer un serveur FTP (vsftpd):**
   ```bash
   sudo apt-get install vsftpd
   ```

7. **Vérifier que le service est actif:**
   ```bash
   sudo systemctl status vsftpd
   ```

8. **Identifiez le socket utilisé par `vsftpd`

En utilisant la command `ss`, identifiez le socket utilisez par `vsftpd` en sachant les informations suivantes : 
- Par défaut, le service va écouter sur le port 21 
- ftp utilise le protocole de transport TCP

9. Effectuez un transfert

Utilisez WinSCP pour vous connecter à votre serveur `ftp` à partir de Windows. 

Transférez un fichier de votre système Windows vers Linux.

Validez que le fichier a bien été transféré.

### Partie B: Modifier la configuration de FTP

10. **Modifier le fichier de configuration FTP:**
   ```bash
   sudo nano /etc/vsftpd.conf
   ```

   Cherchez et modifiez les paramètres suivants:
   - Ajouter ou modifier le port: `listen_port=2100` (exemple)
   - Redémarrez le service ftp à l'aide de la commande `sudo systemctl restart vsftpd`
   - Validez à l'aide de `ss` le changement de configuration.
   - Faite un nouveau transfert de fichier à partir de votre système Windows. Les paramètres de connexion vont devoir changer pour accomoder le changement de configuration.

11. **Changer l'adresse d'écoute:**
   - Ajouter ou modifier l'adresse d'écoute: `listen_address=127.0.0.1` 
   - Redémarrez le service et valider le changement à l'aide de `ss`
   - Essayez de faire un nouveau transfert de fichier à partir de Windows. Est-ce que ça fonctionne? Qu'est-ce qui vous en empêche?

12. **Changer l'adresse d'écoute #2**
   - Ajouter ou modifier l'adresse d'écoute en la remplaçant par votre adresse IP.
   - Redémarrez le service et valider le changement à l'aide de `ss`
   - Essayez de faire un nouveau transfert de fichier à partir de Windows. Est-ce que ça fonctionne?

13. **Changer l'adresse d'écoute #3**
   - Ajouter ou modifier l'adresse d'écoute: `listen_address=0.0.0.0` 
   - Redémarrez le service et valider le changement à l'aide de `ss`
   - Essayez de faire un nouveau transfert de fichier à partir de Windows. Est-ce que ça fonctionne?  Qu'est-ce que le `0.0.0.0` représente ici?

---

## Exercice 3: Installation et configuration d'Avahi (mDNS)

### Partie A: Installer Avahi

18. **Sur la machine Linux, installer Avahi:**
    ```bash
    sudo apt install avahi-daemon avahi-utils
    ```

19. **Vérifier que le service est actif:**
    ```bash
    sudo systemctl status avahi-daemon
    ```


### Partie B: Configurer le nom d'hôte mDNS

21. **Vérifier ou modifier le nom d'hôte Linux:**
    ```bash
    hostname
    # Pour modifier:
    sudo hostnamectl set-hostname mon-serveur-linux
    
    sudo systemctl restart avahi-daemon
    ```

Vérifiez le changement de nom d'hôte à l'aide de la commande `hostname`

21. **Vérifier que le domaine .local est maintenant accessible:**
    ```bash
    # Depuis Linux:
    ping mon-serveur-linux.local
    
    # Depuis Windows (cmd ou PowerShell):
    ping mon-serveur-linux.local
    ```

### Partie C: Le fichier /etc/avahi/avahi-daemon.conf

Modifiez le fichier `/etc/avahi/avaha-daemon.conf` et ajoutez y la ligne 

```bash
host-name=foo
```

Redémarrez le service avahi-daemon.

À partir de Windows, essayez de rejoindre : 
- mon-serveur-linux.local
- foo.local

Que remarquez-vous?

Validez que les changements sont persistents après un redémarrage de la machine (`hostname` et configuration `avahi`)

Reconfigurez votre système de façon à pouvoir le rejoindre par un nom d'hôte unique. 

---

## Exercice 5: Tester les services avec le nom mDNS

### Partie A: Se reconnecter à Telnet via mDNS

23. **Depuis Windows, relancer Putty avec le nom .local:**
    - Lancez Putty à nouveau
    - Au lieu de saisir l'adresse IP, saisissez: le nom d'hôte configuré .local
    - Port: le port modifié si vous l'avez changé (sinon 23)
    - Type: Telnet
    - Connectez-vous

**Questions:**
1. Avez-vous pu vous connecter en utilisant le nom .local au lieu de l'adresse IP?
2. Est-ce plus facile à retenir que l'adresse IP?

### Partie B: Se reconnecter à FTP via mDNS

24. **Depuis Windows, relancer WinSCP avec le nom .local:**
    - Lancez WinSCP
    - Nom d'hôte: Utilisez le nom d'hôte configuré .local
    - Port: le port modifié si vous l'avez changé (sinon 21)
    - Protocole: FTP
    - Connectez-vous

**Questions:**
3. Avez-vous pu vous connecter au service FTP en utilisant le nom .local?

---

## Exercice 6: Résolution de noms et limitations de nslookup

### Partie A: Tester la résolution avec ping

25. **Sur Windows, tester la résolution du nom .local:**
    ```cmd
    ping nom-hote.local
    ```

26. **Vérifier l'adresse IP retournée:**
    - Quelle adresse IP a été retournée?
    - Correspond-elle à l'adresse IP de votre serveur Linux?

**Questions:**
1. Le nom .local a-t-il été correctement résolu avec ping?
2. Quel protocole ping utilise-t-il pour résoudre le nom?

### Partie B: Tester avec nslookup

27. **Sur Windows, essayer de résoudre le nom avec nslookup:**
    ```cmd
    nslookup nom-hote.local
    ```

28. **Observer le résultat:**
    - Quelle réponse obtenez-vous?
    - Est-ce la même que celle obtenue avec ping?

**Questions:**
3. nslookup a-t-il pu résoudre le nom .local? Qu'a-t-il répondu?
4. Pourquoi nslookup ne peut-il pas résoudre les noms mDNS (.local)?
5. Quelle est la différence entre la résolution de noms DNS standard et mDNS?

