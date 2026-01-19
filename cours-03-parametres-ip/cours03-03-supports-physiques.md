# Les câbles et standards Ethernet
## Les types de câbles

<img src="img/Pasted image 20260115125017.png" width="800" />


<img src="img/Pasted image 20260115125122.png" width="800" />

## Le connecteur RJ-45

<img src="img/Pasted image 20260115125217.png" width="800" />

## Les standards Ethernet

**Distance maximale** : 100 mètres

Différents standards viennent définir les mécanismes de transmission sur un même câble. Ces différents standards permettent d'atteindre des bandes-passantes différentes.

Pour qu'un standard puisse être utilisé, il est nécessaire que tous les adapteurs connectés au câbles supportent ce standard.

| Nom        | Nom alternatif   | Bande passante |
| ---------- | ---------------- | -------------- |
| 10BASE-T   | Ethernet         | 10Mbps         |
| 100BASE-T  | Fast Ethernet    | 100Mbps        |
| 1000BASE-T | Gigabit Ethernet | 1000Mbps       |
#### Pour avoir une idée de ce qui est supporté par notre interface :

**Sur Windows** : Regarder le champs description retourner par `ipconfig /all`
**Sur Linux** : Regarder la description retourner par `lspci`

## Connexion d'un câble Ethernet

### Connexion

<img src="img/Pasted image 20260115133342.png" width="800" />

**Attention!** Assurez-vous d'appuyer sur la languette pour bien retirer le câble. Si la languette est endommagée, il est possible que le câble se déconnecte de lui-même.
### Lumières de status et d'activité

<img src="img/Pasted image 20260115130703.png" width="800" />
Pour que la lumière de status soit allumée, les conditions suivantes doivent être remplies : 
- Les deux extrémités du câble doivent être connectées à des appareils
- Les appareils doivent être opérationnels
- Les interfaces doivent être actifs sur les appareils

<hr>

[Précédent](cours03-02-donnees-et-transfert.md) - [Suivant](cours03-04-param-ip.md)
