# Service

## Qu'est-ce qu'un service

Un **service** (ou daemon sous Linux) est un programme qui s'exécute en arrière-plan de manière continue pour fournir une fonctionnalité spécifique au système ou aux utilisateurs. Les services démarrent généralement au démarrage du système et fonctionnent sans interaction directe avec l'utilisateur.

Exemples de services courants : serveur web (apache2, nginx), serveur SSH (sshd), serveur de base de données (mysql, postgresql), serveur DHCP, etc.

### systemd

**systemd** est le système d'initialisation (init) et gestionnaire de services par défaut sur la plupart des distributions Linux modernes (Ubuntu, Debian, Fedora, Arch, etc.). Il remplace l'ancien système SysV init.

Rôles principaux de systemd :
- Démarrer le système et ses services
- Gérer les services (démarrer, arrêter, redémarrer)
- Gérer les dépendances entre services
- Surveiller l'état des services
- Journaliser les événements système

## systemctl

`systemctl` est la commande principale pour contrôler systemd et gérer les services.

### Commandes de base

**Gérer l'état d'un service :**
```bash
# Démarrer un service
sudo systemctl start nom-service

# Arrêter un service
sudo systemctl stop nom-service

# Redémarrer un service
sudo systemctl restart nom-service

# Recharger la configuration (sans redémarrer)
sudo systemctl reload nom-service
```

**Gestion du démarrage automatique :**
```bash
# Activer un service au démarrage
sudo systemctl enable nom-service

# Désactiver un service au démarrage
sudo systemctl disable nom-service

# Activer et démarrer immédiatement
sudo systemctl enable --now nom-service
```

**Vérifier l'état des services :**
```bash
# Afficher l'état détaillé d'un service
systemctl status nom-service

# Vérifier si un service est actif
systemctl is-active nom-service

# Vérifier si un service est activé au démarrage
systemctl is-enabled nom-service

# Lister tous les services actifs
systemctl list-units --type=service --state=running

# Lister tous les services (actifs et inactifs)
systemctl list-units --type=service --all
```

### Exemples pratiques

```bash
# Gérer le serveur SSH
sudo systemctl status sshd
sudo systemctl restart sshd
sudo systemctl enable sshd

# Gérer le serveur web Apache
sudo systemctl start apache2
sudo systemctl enable apache2
systemctl status apache2

# Gérer le service réseau
sudo systemctl restart networking
systemctl status networking

# Voir les services qui ont échoué
systemctl --failed
```

## journalctl

`journalctl` est la commande pour consulter les journaux (logs) de systemd. Tous les messages des services et du système sont centralisés dans le journal systemd.

### Commandes de base

**Consulter les journaux :**
```bash
# Afficher tous les journaux
journalctl

# Afficher les journaux en temps réel (comme tail -f)
journalctl -f

# Afficher les journaux depuis le dernier démarrage
journalctl -b

# Afficher les journaux d'un démarrage précédent (-1 = avant-dernier)
journalctl -b -1
```

**Filtrer par service :**
```bash
# Journaux d'un service spécifique
journalctl -u nom-service

# Journaux SSH en temps réel
journalctl -u sshd -f

# Journaux Apache
journalctl -u apache2
```

**Filtrer par temps :**
```bash
# Depuis une date/heure
journalctl --since "2026-02-11 10:00:00"

# Depuis aujourd'hui
journalctl --since today

# Depuis hier
journalctl --since yesterday

# Entre deux dates
journalctl --since "2026-02-10" --until "2026-02-11 12:00"

# Dernière heure
journalctl --since "1 hour ago"
```

**Filtrer par priorité :**
```bash
# Seulement les erreurs et plus grave
journalctl -p err

# Niveaux : emerg, alert, crit, err, warning, notice, info, debug
journalctl -p warning
```

**Contrôler l'affichage :**
```bash
# Afficher les messages les plus récents d'abord
journalctl -r

# Limiter le nombre de lignes
journalctl -n 50

# Sans pagination (tout afficher d'un coup)
journalctl --no-pager

# Format détaillé avec toutes les métadonnées
journalctl -o verbose
```

### Exemples pratiques

```bash
# Suivre les erreurs SSH en temps réel
journalctl -u sshd -p err -f

# Voir les erreurs d'aujourd'hui
journalctl -p err --since today

# Analyser pourquoi un service a échoué
systemctl status nom-service
journalctl -u nom-service -n 100

# Vérifier ce qui s'est passé au dernier démarrage
journalctl -b -p warning

# Voir l'activité réseau récente
journalctl -u networking --since "10 minutes ago"

# Nettoyer les anciens journaux (garder 2 jours)
sudo journalctl --vacuum-time=2d

# Limiter la taille totale des journaux (garder max 500M)
sudo journalctl --vacuum-size=500M
```

### Combinaison systemctl et journalctl

```bash
# Workflow typique de diagnostic d'un service
systemctl status apache2           # État actuel
journalctl -u apache2 -n 50        # Derniers messages
sudo systemctl restart apache2     # Redémarrer
journalctl -u apache2 -f           # Surveiller en temps réel
```
