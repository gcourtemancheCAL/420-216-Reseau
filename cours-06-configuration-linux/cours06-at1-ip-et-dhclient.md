# Atelier formatif — Configuration IP temporaire avec `ip` et `dhclient`

## Objectif général

L'objectif de cet atelier consiste à se familiariser à l'utilisation des commandes `ip` et `dhclient` dans un contexte dans lequel nous allons vouloir obtenir des informations sur notre système mais aussi modifier les paramètres actuels de notre système.

Cet atelier peut être réalisé seul ou en groupe. 

**Si vous réalisé l'atelier en groupe :** assurez-vous de travailler de façon à ce que tous les membres de l'équipe puisse pratiquer la matière - vous devrez (tous et chacun) être capable d'effectuer ces commandes sans référence rendu en évaluation.

**À la fin de cet atelier vous devrez** :
- Savoir la forme générale d'une commande `ip`
- Être capable d'utiliser la commande `ip` afin de consulter l'état des adresses et des liens.
- Être capable d'activer et désactiver un lien via la commande `ip`
- Être capable de modifier les adresses (ajouter et supprimer) associées à une interface via la commande `ip`
- Être capable de demande une configuration IP via dhcp en utilisant la commande `dhclient`
- Être capable de relâcher une configuration IP via dhcp en utilisant la commande `dhclient`
- Être capable d'utiliser la commande `ping` afin de valider l'état du réseau
- Comprendre que ces configurations sont non persistantes.

## Mise en place
Cet atelier est à réaliser sur une machine virtuelle sur laquelle vous aurez préalablement installer Debian. Des instructions spécifiques vous ont été fournies à la séance précédente. 

**Instructions de mise en place :**
- Connectez, par câble, votre ordinateur portable au routeur mis à votre disposition.
- Configurez votre machine virtuelle en mode réseau "par pont" ou "bridged adapter". Vous devrez - à cet étape - sélectionner l'adaptateur physique avec lequel vous allez effectuer un pont. Sélectionnez votre adaptateur ethernet.
- Démarrez votre machine virtuelle.
- Une fois la machine virtuelle démarrée, testez la connectivité au réseau à l'aide de la commande `ping -c 4 192.168.100.1`

**Configuration réseau de la machine virtuelle**

<img src="img/Pasted image 20260202092105.png" width="500" />

---

## Partie A — Découverte de l’état réseau

Pour cette partie, il est assumer que votre système dispose de la configuration d'interface par défaut. Dans cette configuration, votre système va automatique se présenter comme client DHCP.

### A1 — Afficher les adresses IP

En utilisant la commande apropriée, affichez les paramètres IP de vos interfaces.

**Général**
1. Quels interfaces sont disponibles sur votre système?
2. Sur la base de leur nom identifier de quel genre d'interface il s'agit (interface de loopback, interface ethernet, interface wifi, intégré à la carte mère, carte d'extension, interface virtuelle).

**Interface de loopback**
1. Quelle est l'adresse IP de l'interface de loopback? 
2. D'où vient cette adresse?

**Interface active**
*Par **interface active** on veut dire l'interface connectée au réseau.*
1. Quel est le nom de l'interface active?
2. Quelle est l'adresse IP de cette interface?
3. Quelle est l'adresse réseau de cette interface?

---

### A2 — Afficher les informations de liaison
En utilisant la commande apropriée, affichez l’état des liens (UP/DOWN) et l’adresse MAC.

**Interface active**
1. Quel est l'état du lien de l'interface active?
2. Quelle est son adresse MAC?

---

### A3 — Afficher les de routing

En utilisant la commande apropriée, identifiez la passerelle par défaut de votre système.

**Passerelle par défaut**
1. Quelle est l'adresse IP de la passerelle par défaut?
2. En utilisant votre masque réseau, calculez l'adresse réseau de votre passerelle.
3. Est-ce que votre passerelle par défaut est dans le même réseau que vous? Est-ce que le contraire aurait du sens? Pourquoi?

---

## Partie B — Manipulations temporaires avec `ip`

> **Important :** Toutes les modifications ici sont temporaires et disparaîtront après un redémarrage.

### B1 — Désactiver et réactiver une interface
1. Utilisez `ping` pour tester la connectivité avec votre passerelle par défaut.
2. Désactivez l’interface active.
3. Vérifiez qu’elle est maintenant DOWN.
4. Utilisez `ping` pour tester la connectivité avec votre passerelle par défaut. 
	1. **Question** : Êtes-vous en mesure de rejoindre la passerelle? Pourquoi?
5. Réactivez l’interface.
6. Vérifiez qu’elle est UP.
	1. Vous devriez maintenant pouvoir rejoindre la passerelle avec `ping`

---

### B2 — Ajouter une adresse IP statique secondaire
1. Ajoutez une **adresse IPv4 secondaire** sur l’interface active (ex. `192.168.100.110/24`).
	1. Pour éviter les conflits avec le DHCP, utilisez une adresse entre `192.168.100.100` et  `192.168.100.200`
	2. Validez avec vos voisins de rangée que vous n'utilisez pas les mêmes adresses.
2. Vérifiez qu’elle est bien ajoutée.
3. À partir d'un autre système (votre ordinateur portable ou la machine virtuelle d'un coéquipié) : 
	1. Essayez de rejoindre votre machine virtuelle via sa nouvelle adresse IP.
	2. Essayez de rejoindre votre machine virtuelle via son autre adresse IP.


---

### B3 — Retirer l’adresse IP secondaire
1. Retirez l’adresse ajoutée à l’étape précédente.
2. Vérifiez qu’elle a disparu.
3. À partir d'un autre système (votre ordinateur portable ou la machine virtuelle d'un coéquipié) : 
	1. Essayez de rejoindre votre machine virtuelle via sa nouvelle adresse IP.
	2. Essayez de rejoindre votre machine virtuelle via son autre adresse IP.


---

### B4 — Remplacer l’adresse IP principale
1. Remplacez l’adresse IP principale par une autre adresse valide du même sous-réseau.
2. Vérifiez le changement.
3. **Testez la connectivité** vers la passerelle (ping).

---

### B5 — Modification de la passerelle
1. Essayez de rejoindre `www.google.com` via ping. 
	1. Vers quelle adresse IP est-ce que `ping` envoie ses messages ICMP?
	2. En utilisant votre masque réseau, calculez l'adresse réseau de `www.google.com`
	3. Est-ce que `www.google.com` est dans le même réseau que vous? Qu'est-ce que cela implique au niveau de la liaison (couche 2)?
2. Essayez de ping les systèmes suivant en utilisant leur adresses IP :
	1. `www.google.com`
	2. Votre routeur (le 192.168.100.1)
3. Supprimez la passerelle par défaut de votre système.
4. Essayez de ping les systèmes suivant en utilisant leur adresses IP :
	1. `www.google.com`
	2. Votre routeur (le 192.168.100.1)
5. Qu'est-ce qui se passe? Pourquoi êtes vous capable de rejoindre l'un des systèmes mais pas l'autre?
6. Ajoutez la précédente passerelle par défaut.

---

### B6 — Supprimer toutes les adresses IP
1. Supprimez toutes les adresses IPv4 de l’interface.
2. Vérifiez qu’il n’y a plus d’IPv4.
3. Essayez de rejoindre la passerelle avec ping. Qu'est-ce qui se passe?


---

## Partie C — DHCP avec `dhclient`

### C1 — Obtenir une configuration DHCP
1. Utilisez `dhclient` pour obtenir une nouvelle configuration.
2. Vérifiez les paramètres IP qui vous ont été assignés (adresse,  réseau, passerelle). Est-ce que ce sont les mêmes que précédemment?

---

### C2 — Relâcher une configuration DHCP
1. Libérez l’adresse DHCP.
2. Vérifiez que l’adresse IP a été retirée. 

---

### C3 — Renouveler une configuration DHCP
1. Renouvelez la configuration DHCP.
2. Vérifiez les paramètres IP qui vous ont été assignés (adresse,  réseau, passerelle). Est-ce que ce sont les mêmes que précédemment?

---

## Partie D — Validation de la non‑persistance

1. Ajoutez une nouvelle adresse IP à votre interface.
2. Redémarrez la machine.
3. Est-ce que vos changements sont encore là?

---

## Aide-mémoire

- Vérifier les interfaces :
  - `ip link show`
  - `ip -br link`
- Vérifier les adresses :
  - `ip address show`
  - `ip -br addr`
- Activer/désactiver une interface :
  - `sudo ip link set <interface> up`
  - `sudo ip link set <interface> down`
- Ajouter une IP :
  - `sudo ip address add <ip>/<masque> dev <interface>`
- Supprimer une IP :
  - `sudo ip address del <ip>/<masque> dev <interface>`
  - `sudo ip address flush dev <interface>`
- DHCP :
  - `sudo dhclient <interface>`
  - `sudo dhclient -r <interface>`

---
