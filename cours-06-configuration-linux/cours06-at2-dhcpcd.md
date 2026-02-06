# Atelier formatif — DHCP avec `dhcpcd`

## Objectif general

L'objectif de cet atelier consiste a se familiariser a l'utilisation de `dhcpcd` pour obtenir, relacher et renouveler une configuration IP via DHCP.

Cet atelier peut etre realise seul ou en groupe.

**A la fin de cet atelier vous devrez** :
- Etre capable de demander une configuration IP via DHCP avec `dhcpcd`
- Etre capable de relacher une configuration IP via DHCP avec `dhcpcd`
- Etre capable de renouveler une configuration IP via DHCP avec `dhcpcd`
- Etre capable de verifier l'etat de votre configuration IP

---

## Partie A — DHCP avec `dhcpcd`

### A0 — Désactiver l'interface

1. Désactivez l'interface via la commande `ifdown`
2. Validez à l'aide de la commande `ip` que l'interface est bien désactivée.

---

### A1 — Obtenir une configuration DHCP
1. Utilisez `dhcpcd` pour obtenir une nouvelle configuration sur l'interface active.
2. Verifiez les parametres IP qui vous ont ete assignes (adresse, reseau, passerelle). Est-ce que ce sont les memes que precedemment?

---

### A2 — Relacher une configuration DHCP
1. Relachez l'adresse DHCP pour l'interface active.
2. Verifiez que l'adresse IP a ete retiree.

---

### A3 — Renouveler une configuration DHCP
1. Renouvelez la configuration DHCP.
2. Verifiez les parametres IP qui vous ont ete assignes (adresse, reseau, passerelle). Est-ce que ce sont les memes que precedemment?

---

### A4 — Redémarrer l'ordinateur

Nous allons redémarrer l'ordinateur afin de remettre le système dans son état initial pour les prochains exercices.

## Aide-memoire

- Verifier les adresses :
  - `ip address show`
  - `ip -br addr`
- DHCP avec `dhcpcd` :
  - `sudo dhcpcd -4 <interface>`
  - `sudo dhcpcd -k <interface>`
  - `sudo dhcpcd -n <interface>`

---
