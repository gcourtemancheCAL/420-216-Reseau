# Atelier : Configuration IP sur Windows

## Préalables
- Activité à réaliser en classe, en équipe de deux.
- Chaque membre se connecte physiquement (câble Ethernet) à un routeur dédié.
- Permissions administrateur requises pour certaines commandes.
- À la fin, remettez vos paramètres IP à leur état initial.

---

## Étape 1 — Connexion des machines au réseau

**Objectifs**
- Connecter les équipements au routeur
- Obtenir une configuration IP via DHCP
- Valider la connectivité entre les systèmes

**Procédure**
- Branchez le câble réseau entre votre PC et le routeur.
- Vérifiez les DEL sur le port du routeur et de la carte réseau.
- Assurez-vous que le profil réseau Windows est « Réseau privé ».

<img src="img/Pasted image 20260119093755.png" width="600" />

**Commandes utiles (Windows)**
```powershell
# Voir le profil réseau et l’interface active (powershell)
Get-NetConnectionProfile

# Voir la configuration IP attribuée (DHCP)
ipconfig /all
```

**Attendus**
- Votre IPv4 doit être dans la plage `192.168.100.100`–`192.168.100.199`.
- La passerelle par défaut doit être `192.168.100.1`.

**Validation de connectivité**
```powershell
# Tester la connectivité vers le routeur
ping 192.168.100.1

# Tester la connectivité vers l'autre système
ping 192.168.100.X

# Tester la connectivité vers un nom de domaine (résolution DNS + accès internet)
ping www.google.com

# Alternative avec plus de détails (powershell)
Test-NetConnection 192.168.100.1
```

---

## Étape 2 — Manipulation du DHCP

**Objectifs**
- Observer et manipuler la configuration obtenue par DHCP
- Comprendre le bail DHCP (obtention/expiration)

**Identification des paramètres (adresse, passerelle, DNS, bail)**
```powershell
# Détails complets, incluant DHCP Enabled, Lease Obtained, Lease Expires
ipconfig /all
```

**Libérer et renouveler l’adresse DHCP**
```powershell
# Libère l’adresse IP DHCP (coupe momentanément la connectivité)
ipconfig /release

# Demande une nouvelle adresse au serveur DHCP
ipconfig /renew
```

**Questions**
- Quels sont « Lease Obtained » et « Lease Expires » avant/après le renouvellement?
- Les paramètres (IPv4, passerelle, DNS) ont-ils changé? Expliquez.

**Validation**
- Les deux systèmes doivent pouvoir se joindre par `ping` après le renouvellement.

---

## Étape 3 — Configuration IP statique

**Objectifs**
- Configurer une interface en IPv4 statique
- Valider la nouvelle configuration et ses impacts

**Procédure**
En utilisant l'une des méthodes de configuration IP vues en classe, configurez statiquement votre interface ethernet connecté au réseau avec les paramètres suivant : 
- IPv4: 192.168.100.50
- Masque: 255.255.255.0 (/24)
- Passerelle: 192.168.100.1
- DNS: 192.168.100.1

**Validation**
```powershell
ipconfig
ping 192.168.100.1
```

**Question**
- Que se passe-t-il si vous lancez `ipconfig /release` et `ipconfig /renew` après avoir mis l’IP statique? Pourquoi?

---

## Étape 4 — Collision d’adresse IP

### Note importante : il est possible que cette étape ne soit pas réalisable. Windows essaie de détecter et prévenir les collision d'adresse.

**Objectifs**
- Observer une erreur classique de configuration: duplication d’IP

**Procédure**
- Mettez les deux machines sur la même IP statique: 192.168.100.50/24, passerelle 192.168.100.1, DNS 192.168.100.1.
- Testez la connectivité vers le routeur simultanément sur les deux postes.

**Observation & outils**
```powershell
# Pings consécutifs (Windows)
ping 192.168.100.1 -t
```

**Question**
- Que se passe-t-il? Proposez une hypothèse.

---

## Étape 5 — Réinitialisation de la configuration IP

**Objectif**
- Revenir à une configuration DHCP propre (adresse et DNS automatiques)

**Procédure**
En utilisant l'une des méthodes de configuration IP vues en classe, configurez votre interface de sorte à ce que ses paramètres IP soient obtenus par DHCP.

**Validation**
```powershell
# Vérifier la config finale
ipconfig /all
```

**Attendu final**
- `DHCP Enabled : Yes`
- IPv4 dans la plage DHCP du routeur (ex.: 192.168.100.10–192.168.100.20)
- Passerelle: 192.168.100.1
- DNS: 192.168.100.1

---
## Rappel
À la fin de l’atelier, assurez-vous d’avoir restauré vos paramètres réseau (DHCP) et de valider avec `ipconfig /all`.

## Conseils de dépannage
### ping

Utilisez `ping` pour valider le fonctionnement du réseau. En cas de problème avec le réseau, essayer de rejoindre des hôtes progressivement plus loin : 
- Le routeur, directement
- Un autre hôte dans le même réseau
- Un hôte sur internet

Le résultat de ces tests peut vous éclairer sur le niveau auquel la connexion échoue.

````bash
# Utilisation : ping <adresse de destination>.
# E.g. : 
 ping 192.168.0.1 # Envoie des paquets ICMP vers le 192.168.0.1
 ping www.google.com # Envoie des paquets ICMP vers www.google.com

#'ping' est une commande permettant de valider la connectivité entre deux 
# systèmes. Lorsque vous utilisé la commande 'ping', votre système va envoyer des 
# messages ICMP vers sa destination. Sur réception, le système de destination va 
# répondre avec ses propres paquets ICMP.

# Ce processus permet de valider que le chemin d'aller-retour entre deux systèmes 
# est fonctionnel.

# L'utilitaire 'ping' va afficher le résultat de chaque requête icmp. Les cas de 
# figures suivants sont possibles :
# - Une réponse a été obtenue de l'hôte de destination.
# - Une réponse indiquant une erreur a été obtenue d'un équipement intermédiaire.
# - Aucune réponse n'a été obtenue.

````
### Échec DHCP

Vous pouvez forcer une requête DHCP avec la commande `ipconfig /renew`. 

Si vous n'avez pas de réponse, que votre adresse IP n'est pas dans la plage indiquée ou que votre adresse ip est dans le réseau 169.254.0.0/16, l'allocation DHCP a échoué.

Si l'allocation d'adresse par DHCP échoue, vérifiez les éléments suivants : 
- La connexion physique au routeur.
	- Est-ce que le câble est branché? Est-ce que le routeur et votre PC détectent la connexion (voyants lumineux)?
- Le mode de configuration de votre interface
	- Assurez-vous que votre interface n'est pas configurée statiquement
	- Vérifiez les paramètres de l'adapteur aussi.