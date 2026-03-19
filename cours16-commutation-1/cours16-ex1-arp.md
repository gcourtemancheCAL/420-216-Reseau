# Exercices: ARP et Cache ARP

## Objectifs
- Comprendre le fonctionnement du protocole ARP
- Afficher la cache ARP et comprendre la cache ARP
- Établir le lien entre l'adresse IP et la trame Ethernet

---

## Exercice 1: Exploration de la cache ARP

### Partie A: Afficher la cache ARP

1. **Afficher la cache actuel:**

   **Windows:**
   ```cmd
   arp -a
   ```

   **Linux:**
   ```bash
   ip neigh show
   ```

   - Combien d'entrées voyez-vous?
   - Quelles adresses IP sont présentes? Identifiez leur rôle (passerelle, serveur, autre?)
   - Regardez la colonne "Type": quelles entrées sont marquées "dynamic"? Statiques?
   - Qu'ont en commun les entrées "statiques"?

2. **Analyser une entrée spécifique:**
   - Trouvez l'adresse MAC de votre passerelle

---

## Exercice 2: Génération de trafic ARP

### Partie A: Vider et regénérer la cache

4. **Vider la cache ARP:**

   **Windows:**
   ```cmd
   arp -d *
   arp -a              # Vérifiez qu'il est vide
   ```

   **Linux:**
   ```bash
   sudo ip neigh flush all
   ip neigh show       # Vérifiez qu'il est vide (ou quasi-vide)
   ```

5. **Regénérer les entrées:**
   - Pingez votre passerelle:
   - Afficher la cache ARP à nouveau:
     ```
     arp -a              # Windows
     ip neigh show       # Linux
     ```
   - Nouvelle entrée pour la passerelle? Elle est marquée "REACHABLE" (Linux) ou "dynamic" (Windows)

---
