# Exercices: ping et traceroute

## Objectifs
- Comprendre le fonctionnement d'ICMP
- Utiliser `ping` pour tester la connectivité
- Utiliser `traceroute` pour visualiser le chemin réseau
- Analyser les résultats et identifier les problèmes

---

## Exercice 1: Exploration basique avec ping

### Partie A: Vérifier la connectivité locale

1. **Pinger votre propre machine:**
   ```
   ping 127.0.0.1
   ```
   - Identifiez le TTL.
   - À quel niveau de la pile OSI/quel protocole se situe cette information?

2. **Pinger la passerelle par défaut:**
   - Identifiez votre passerelle par défaut (commande: 
   - Pingez-la
   - Identifiez le TTL.

3. **Pinger un serveur public:**
   ```
   ping 8.8.8.8
   ```
   - Le serveur répond-il? Quel est le temps de réponse moyen?

**Questions**
1. À quoi correspond exactement le TTL dans les réponse reçu? Est-ce celui que vous avez envoyé à l'origine?
2. Estimez le TTL initial pour chacune des réponses reçues.
3. Quel type de message ICMP avez vous envoyé avec ping? Quel type de réponse?

### Partie B: Questions sur le broadcast

4. **Broadcast et ping:**
   - Identifiez votre adresse réseau. 
   - Calculez l'adresse de diffusion.
   - Selon-vous, qu'est-ce qui devrait arriver si l'on tentait de rejoindre cette adresse avec ping?

**NB** : Vous pouvez tester la commande et observer le résultat. Il est fort probable, cependant, [que le routeur bloque les paquets ICMP par mesure de sécurité](https://en.wikipedia.org/wiki/Smurf_attack).

---

## Exercice 2: Analyse du chemin avec traceroute

### Partie A: Tracer le chemin vers des destinations publiques

1. **Vers Google DNS:**
   ```
   traceroute 8.8.8.8          # Linux
   tracert 8.8.8.8             # Windows
   ```
   - Combien de sauts avant d'atteindre la destination?
   - Quel est le premier routeur? Est-ce que cette adresse vous dit quelque chose?
   - Qu'est-ce que ça veut dire lorsque le résultat affiche des `*`?

### Partie B: Comprendre les messages TTL

3. **Observation du TTL:**
   - Lancez avec un ttl de 1
	   - **Linux** : `ping -t 1 8.8.8.8` 
	   - **Windows** : `ping -i 1 8.8.8.8`
   - Quel résultat obtenez-vous?
   - À partir de quelle valeur est-ce que l'opération fonctionne?

4. **Host Unreachable:**
   - Essayez de tracer vers un serveur inexistant sur votre réseau
   - Que remarquez-vous comme résultat?

---

