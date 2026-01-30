# Atelier : Identifier le manufacturier de votre adapteur réseau

## 1. Obtenir l'adresse MAC de votre interface

### Sur Windows :

En terminal : `ipconfig /all`

<img src="img/Pasted image 20260115095311.png" width="900" />

En lisant le nom et la description de l'interface, identifiez l'interface qui vous intéresse. Il est nécessire de prendre le temps de comprendre l'information qui vous est présentée.

Dans mon cas, l'interface qui m'intéresse est le "Wireless LAN adapter-Wi-Fi 2". 
1. Je suis intéressé par l'interface qui est connectée au réseau. C'est la seule qui a des paramètres IP.
2. Les autres interfaces sont *virtuelles*. À moins de spécifiquement m'intéresser à ces interfaces, ce ne sont probablement pas celles que je veux voir.

Mon adresse MAC : **6C-6A-77-30-41-1B**

### Sur Linux :

En terminal : `ip link`

<img src="img/Pasted image 20260115105249.png" width="900" />

Identifiez l'adapteur qui vous intéresse sur la base de son nom. L'adresse MAC est l'adresse "link/ether".

## 2. Identifier le manufacturier

Suivez le lien suivant : https://maclookup.app/

Entrez votre adresse MAC dans le champs correspondant et cliquez sur "search".
