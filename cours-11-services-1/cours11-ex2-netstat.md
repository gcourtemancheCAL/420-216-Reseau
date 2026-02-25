# Exercices: netstat et les sockets TCP/UDP

## Objectifs
- Comprendre la structure des connexions TCP et UDP
- Utiliser `netstat` pour analyser les sockets actifs
- Identifier les adresses locales et distantes
- Analyser les états des connexions TCP
- Associer les sockets aux processus

---

## Exercice 1: TCP - Adresses locales et écoute

### Partie A: Afficher les sockets en écoute

1. **Afficher les sockets TCP actifs:**

   **Windows:**
   ```cmd
   netstat -an
   ```

   - Combien de sockets TCP voyez-vous en état d'écoute (LISTEN)?
   - Regardez le port local utilisé pour chacun de ces sockets. En reconnaissez-vous certains?

2. **Analyser les adresses locales en écoute:**
   - Trouvez toutes les adresses IPv4 qui sont en écoute
   - Listez les adresses que vous trouvez

**Questions:**
1. Parmi les adresses en écoute, trouvez-vous votre adresse IP (celle de votre interface réseau)? Qu'est-ce que cela signifierait?
2. Trouvez-vous l'adresse 127.0.0.1? Qu'est-ce que cela représente?
3. Trouvez-vous l'adresse 0.0.0.0? Qu'est-ce que cela signifie dans ce contexte?

---

## Exercice 2: TCP - Adresses distantes et connexions établies

### Partie A: Analyser les adresses distantes

3. **Observer les connexions établies:**
   - Identifiez les connexions TCP en état LISTENING et ESTABLISHED
   - Notez les adresses distantes (remote address)

**Questions:**
1. Est-ce que les sockets en état LISTENING ont une adresse distante? Que signifie cette adresse?
2. Parmi les adresses distantes, trouvez-vous votre adresse IP? Qu'est-ce que cela signifierait?
3. Trouvez-vous l'adresse 127.0.0.1 comme adresse distante? Qu'est-ce que cela indiquerait?
4. Il y a-t-il un port que vous reconnaissez parmi les adresses distantes?

---

## Exercice 3: UDP - Absence d'états de connexion

### Partie A: Examiner les sockets UDP

4. **Afficher les sockets UDP:**
   ```
   netstat -an | findstr UDP     # Windows
   ```
   - Combien de sockets UDP voyez-vous?
   - Quel état est affiché pour ces sockets?

**Questions:**
1. Quel état ont les sockets UDP lorsqu'ils sont affichées par netstat?
2. Pourquoi les sockets UDP n'ont-ils pas d'état (LISTEN, ESTABLISHED, etc.)?
3. Qu'est-ce que cela révèle sur la nature de ces protocoles (TCP vs UDP)?

---

## Exercice 4: Association des sockets aux processus

### Partie A: Identifier les processus

5. **Afficher les processus avec les sockets:**

   **Windows:**
   ```cmd
   netstat -ano
   ```

   - Identifiez un socket intéressant et son PID associé
   - Notez l'adresse locale et le port

6. **Associer aux processus:**
   
   **Windows:** Ouvrez le Gestionnaire des tâches
   - Allez à l'onglet "Processus"
   - Trouvez le processus correspondant au PID que vous avez noté
   - Identifiez le nom du processus et son application
   
**Questions:**
6. Quel processus utilise le socket que vous avez choisi?
7. Cette application vous est-elle familière? À quoi sert-elle?