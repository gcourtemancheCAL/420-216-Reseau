# Taille des données en réseau

## Préfixes métriques

| Préfixe | Abbréviation | Multiple                     |
| ------- | ------------ | ---------------------------- |
| Kilo    | K            | 1000                         |
| Méga    | M            | 1000 \* 1000                 |
| Giga    | G            | 1000 \* 1000 \* 1000         |
| Téra    | T            | 1000 \* 1000 \* 1000 \* 1000 |

## Préfixes binaires

| Préfixe | Abbréviation | Multiple                     |
| ------- | ------------ | ---------------------------- |
| Kibi    | Ki           | 1024                         |
| Mébi    | Mi           | 1024 \* 1024                 |
| Gibi    | Gi           | 1024 \* 1024 \* 1024         |
| Tébi    | Ti           | 1024 \* 1024 \* 1024 \* 1024 |

## Octets vs Bits

En informatique, on va souvent utiliser l'octet comme unité de base. En réseau, on va souvent préconiser le bit puisque c'est l'unité avec laquelle les supports physiques travaillent.

Il y a 8 bits dans 1 octet.

<img src="img/Pasted image 20260115120728.png" width="500" />


**Les différentes unités de mesure** :

<table>
  <thead>
    <tr>
      <th colspan="2">Métrique</th>
      <th  colspan="2">Binaire</th>
    </tr>
    <tr>
      <th>Octet</th>
      <th>Bit</th>
      <th>Octet</th>
      <th>Bit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>o</td>
      <td>b</td>
      <td>o</td>
      <td>b</td>
    </tr>
    <tr>
      <td>Ko</td>
      <td>Kb</td>
      <td>Kio</td>
      <td>Kib</td>
    </tr>
    <tr>
      <td>Mo</td>
      <td>Mb</td>
      <td>Mio</td>
      <td>Mib</td>
    </tr>
    <tr>
      <td>Go</td>
      <td>Gb</td>
      <td>Gio</td>
      <td>Gib</td>
    </tr>
    <tr>
      <td>To</td>
      <td>Tb</td>
      <td>Tio</td>
      <td>Tib</td>
    </tr>
  </tbody>
</table>

# Transfert de données

Le transfert des données est mesuré en "unité par seconde". 

**Exemple** : 1Mo/s = 1 mégaoctet de données transféré par seconde.

On va souvent mesuré le transfert de données en bits par seconde.

**Exemple** : 1 Mbps = 1 megabit par seconde.

On va préférer la forme "Mbps" à "Mb/s" pour éviter la confusion avec le "megabyte per second" en anglais. Bien entendu, comme toute chose en informatique, il existe plusieurs exemples réels  qui en diffèrent.
### Définitions

- **Bande passante** : Capacité de transfert
- **Débit** : Vitesse réelle de transfert
- **Latence** : Délai de transmission.
- **RTT** : "Round trip time". Le délai pour l'aller retour. **Latence** et **RTT** sont souvent interchangés même si techniquement différents.

<img src="img/Pasted image 20260115121156.png" width="500" />

#### Bande passante 

<img src="img/Pasted image 20260115121245.png" width="200" />

#### Débit 

<img src="img/Pasted image 20260115121315.png" width="200" />

#### Latence

<img src="img/Pasted image 20260115121348.png" width="200" />


<hr>

[Précédent](cours03-01-adapteurs-intf.md) - [Suivant](cours03-03-supports-physiques.md)