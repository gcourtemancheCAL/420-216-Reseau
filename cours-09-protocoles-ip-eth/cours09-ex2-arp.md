# Exercices: ARP et Cache ARP

## Objectifs
- Comprendre le fonctionnement du protocole ARP
- Afficher la cache ARP
- Visualiser les trames ARP à la découverte
- Analyser les adresses multicast et les entrées statiques
- Identifier les risques de sécurité (ARP spoofing)

---

## Exercice 1: Exploration du cache ARP

### Partie A: Afficher le cache ARP

1. **Afficher le cache actuel:**

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

3. **Adresses multicast (224.0.0.0/8):**
   - Vérifiez s'il y a des entrées pour des adresses dans la plage 224.0.0.0/8 dans le cache ARP

---

## Exercice 2: Génération de trafic ARP

### Partie A: Vider et regénérer le cache

4. **Vider le cache ARP:**

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
   - Pinguez votre passerelle:
     ```
     ping 192.168.1.1        # (remplacer par votre passerelle)
     ```
   - Afficher le cache ARP à nouveau:
     ```
     arp -a              # Windows
     ip neigh show       # Linux
     ```
   - Nouvelle entrée pour la passerelle? Elle est marquée "REACHABLE" (Linux) ou "dynamic" (Windows)

5. **Pinguez un hôte:**
   - Sur Linux : 
	   - Vider la cache ARP
	   - Rejoignez l'adresse IP de votre système Windows avec ping.
	   - Affichez la cache ARP. Que remarquez-vous?
	   
5. **Pinguez un hôte inexistant:**
   - Sur Linux : 
	   - Vider la cache ARP
	   - Utilisez ping pour essayer de rejoindre un hôte inexistant.
	   - Affichez la cache ARP. Que remarquez-vous?

### Partie B: Comprendre les requêtes ARP

7. **Broadcast vs Unicast:**
   - Avant de pinger une adresse inconnue dans votre réseau local, quel type de trame Ethernet est envoyé?

---
