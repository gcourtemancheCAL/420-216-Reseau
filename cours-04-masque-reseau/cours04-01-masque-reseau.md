# Adresse et masque réseau

## Adresse réseau et masque

### Poid fort et poid faible

Rappel : la valeur d'un bit dépend de sa position.

| 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 128 | 64  | 32  | 16  | 8   | 4   | 2   | 1   |
On va qualifier de "poid fort" les bits qui ont une plus grosse valeur, et de "poid faible" les bits qui ont une valeur plus faible.

Donc, dans cet exemple, si je parles des 4 bits de poid fort, je parles des 4 bits les plus à gauche.

Le même principe s'applique sur les octets.

Dans une adresse IP, l'octet le plus à gauche est l'octet de poid fort et l'octet le plus à droite est l'octet de poid faible.

**Exemple** : `127.196.0.222`
	L'octet de poid fort est le `127`
	L'octet de poid faible est le `222`

### Représentation binaire d'une adresse IP

Exemple : 192.168.0.1

On converti chaque octet en forme binaire
- `192 -> 1100 0000`
- `168 -> 1010 1000`
- `0 -> 0000 0000`
- `1 -> 0000 0001`

On les concatène en ordre
`1100 0000 1010 1000 0000 0000 0000 0001`

On peut garder les points pour s'aider à mieux différencier les octets
`1100 0000 . 1010 1000 . 0000 0000 . 0000 0001`

#### Conversion rapide d'un octet

Vous avez vu une méthode en mathématique qui fonctionne pour n'importe quel nombre mais qui peut être longue à appliquer.

Je vous propose une méthode plus rapide dans le contexte de conversion d'un octet. 

La méthode fonctionne par soustraction successive. 
- On commence par le bit de poid fort et on se déplace vers le bit de poid faible.
- Pour chaque bit, on regarde si on peut le soustraire à notre valeur courante. Si oui, le bit est à 1. Sinon, il est à 0.
- À chaque soustraction on met à jour notre valeur courante.

**Exemple 1** : Conversion de 220 
1. Est-ce qu'on peut soustraire 128 de 220? Oui -> Bit = 1, Val = 220 -128 = 92
2. Est-ce qu'on peut soustraire 64 de 92? Oui -> Bit = 1, Val = 92 - 64 = 28
3. Est-ce qu'on peut soustraire 32 de 28? Non -> Bit = 0, Val = 28
4. Est-ce qu'on peut soustraire 16 de 28? Oui -> Bit = 1, Val = 28 - 16 = 12
5. Est-ce qu'on peut soustraire 8 de 12? Oui -> Bit = 1, Val = 12 - 8 = 4
6. Est-ce qu'on peut soustraire 4 de 4? Oui -> Bit = 1, Val = 4 - 4 = 0
7. Est-ce qu'on peut soustraire 2 de 0? Non -> Bit = 0, Val = 28
8. Est-ce qu'on peut soustraire 1 de 0? Non -> Bit = 0, Val = 28

**Résultat : 1101 1100**

**Exemple 2** : Conversion de 117 
1. Est-ce qu'on peut soustraire 128 de 117? Non -> Bit = 0, Val = 117
2. Est-ce qu'on peut soustraire 64 de 117? Oui -> Bit = 1, Val = 117 - 64 = 53
3. Est-ce qu'on peut soustraire 32 de 53? Non -> Bit = 0, Val = 53 - 32 = 21
4. Est-ce qu'on peut soustraire 16 de 21? Oui -> Bit = 1, Val = 21 - 16 = 5
5. Est-ce qu'on peut soustraire 8 de 5? Non -> Bit = 0, Val = 5
6. Est-ce qu'on peut soustraire 4 de 5? Oui -> Bit = 1, Val = 5 - 4 = 1
7. Est-ce qu'on peut soustraire 2 de 1? Non -> Bit = 0, Val = 28
8. Est-ce qu'on peut soustraire 1 de 1? Oui -> Bit = 1, Val = 1 - 1 = 0

**Résultat : 0101 0101**

[Vidéo de la méthode](https://www.youtube.com/watch?v=OXj_-dyKsGI)
### Masques binaires

Un masque binaire est un outil fréquemment utilisé en informatique afin d'isoler une information statique d'une donnée binaire variable.

Le but d'un masque est de permettre cette forme de test d'égalité : 
`VALEUR_VARIABLE & MASQUE == CIBLE`

En réseautique, cet outil va-t-être utilisé à plusieurs fin :
- Identifier la partie réseau d'une adresse IP.
- Identifier les routes à prendre.
- Identifier les permissions sur les hôtes.
- Identifier les hôtes concernés par des règles de pare-feu.
- ...

L'application d'un masque binaire repose sur l'application et les propriétés du "ET" binaire.

| A   | &   | B   | =   | R   |
| --- | --- | --- | --- | --- |
| 0   | &   | 0   | =   | 0   |
| 1   | &   | 0   | =   | 0   |
| 0   | &   | 1   | =   | 0   |
| 1   | &   | 1   | =   | 1   |

Le résultat de l'opération `A & B` va être `1` uniquement si `A` et `B` sont à `1` aussi. Hors donc, lorsqu'une valeur est fixé, les égalités suivantes vont toujours être vraies : 
- `A & 1 = A` 
- `A & 0 = 0`

Notre masque va donc être construit de sorte à ce que tous les bits importants (ceux que l'on veut conserver) soient à 1, et les bits que l'on veut ignorer vont-être à 0.

Ainsi, en appliquant notre masque, les bits importants vont rester identique et les autres vont être normalisés à 0.

**Exemple de masque réseau** 

**Adresse** : 192.168.0.1 **Masque** : 255.255.0.0

**Opération** : 192.168.0.1 & 255.255.0.0

**Application** : 
`Addresse en binaire : 1100 0000 . 1010 1000 . 0000 0000 . 0000 0001`
`Masque en binaire   : 1111 1111 . 1111 1111 . 0000 0000 . 0000 0000`
`Résultat du &       : 1100 0000 . 1010 1000 . 0000 0000 . 0000 0000`

Ici, l'adresse réseau serait donc le `192.168.0.0`.

### Masques réseau

**Quelques définitions**
- **Adresse réseau** : Adresse identifiant le réseau dans lequel on se trouve. L'adresse est réseau est utilisée pour savoir si le destinataire d'un message est dans le même réseau que nous ou bien si le message doit être routé par la passerelle.
- **Partie réseau de l'adresse** : Partie de l'adresse IP qui sert à identifier l'adresse réseau. 
- **Partie hôte de l'adresse** : Partie de l'adresse IP qui sert à identifier un hôte dans un réseau.

Lorsque l’on assigne une adresse IP à un hôte, on lui assigne aussi un masque de réseau. 

Le masque identifie la partie binaire de l’adresse IP servant à identifier le réseau. C’est toujours une séquence contiguë de bits partant de l’extrême gauche.

**Exemple 1** 

<img src="img/Pasted image 20260115153707.png" width="800" />

**Exemple 2** 

<img src="img/Pasted image 20260115153934.png" width="800" />

### Notation CIDR

<img src="img/Pasted image 20260115160128.png" width="800" />

<img src="img/Pasted image 20260115160209.png" width="800" />
#### CIDR : Raccourci par octet
| 0   | 0   | 0000 0000 |
| --- | --- | --------- |
| 1   | 128 | 1000 0000 |
| 2   | 192 | 1100 0000 |
| 3   | 224 | 1110 0000 |
| 4   | 240 | 1111 0000 |
| 5   | 248 | 1111 1000 |
| 6   | 252 | 1111 1100 |
| 7   | 254 | 1111 1110 |
| 8   | 255 | 1111 1111 |
