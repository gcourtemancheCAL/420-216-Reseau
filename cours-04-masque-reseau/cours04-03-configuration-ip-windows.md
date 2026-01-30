## Configuration IP sur Windows

Dans les paramètres système, choisissez l'onglet "Réseau et internet" : 

<img src="img/Pasted image 20260115173049.png" width="600" />

Dans cet onglet, sélectionnez l'interface à configurer.

### Profile de réseau

Le profile de réseau indique le profile de sécurité à appliquer par défaut. 

Un réseau dit "publique" est un réseau auquel n'importe qui peut se connecter. Dans ce contexte, des mesures de sécurité supplémentaires sont appliquées.

Un réseau "privée" est un réseau dans lequel seulement des hôtes connus vont pouvoir se connecter. C'est un réseau, normalement, considéré de confiance. Dans ce contexte, moins de mesures de sécurité sont appliquées.

**Dans le contexte de ce cours, nous allons toujours choisir un profile de type "réseau privé"**

<img src="img/Pasted image 20260115173558.png" width="600" />

### Configuration IP

Dans ce menu vous pouvez choisir le mode de configuration IP de votre interface : statique ou dynamique. Ici, ma configuration est dynamique.

<img src="img/Pasted image 20260115174005.png" width="600" />

Ici, je configure statiquement mes paramètres IP :

<img src="img/Pasted image 20260115174154.png" width="600" />

### WiFi

Pour une connexion WiFi, vous allez devoir sélectioner le réseau auquel vous êtes connecté.

<img src="img/Pasted image 20260115173405.png" width="600" />

## Configuration IP sur Windows - Méthode alternative
### IMPORTANT!!!!

**Bien que cette méthode dispose de certains avantages par rapport à l'autre, il est important d'y faire attention.** Cette méthode va avoir **préseance** sur la configuration de Windows. **De plus,** cette méthode **ne sera pas visible** à partir des paramètres Windows.

1. À partir du `Panneau de configuration` - `Réseau et partage`

<img src="img/Pasted image 20260126090314.png" width="800" />

2. Dans la barre de gauche - `Modifier les paramètres de la carte`

<img src="img/Pasted image 20260126090559.png" width="800" />

3. Sur votre adapteur - clique droit et `Propriétés`

<img src="img/Pasted image 20260126090744.png" width="800" />

4. Sélectionnez la version du protocole IP que vous voulez configurer (dans notre cas, IPv4) et cliquez sur `Propriétés`.

<img src="img/Pasted image 20260126090849.png" width="400" />

5. Vous pouvez maintenant configurer les paramètres IP de l'adapteur.

<img src="img/Pasted image 20260126091106.png" width="400" />

**Important** : Les changements vont seulement prendre effet une fois la fenêtre des paramètres de l'adapteur fermée (on l'a ouverte à l'étape 3, et on en voit une capture d'écran à l'étape 4). 


<hr>

[Précédent](cours04-02-dhcp.md)


