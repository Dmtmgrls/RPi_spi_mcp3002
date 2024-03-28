### ETUDE DU PACKAGE $spidev$ ⭐ ⭐

>[!IMPORTANT]
>  Ce document est un résumé des documents suivants :<br>
>-  [En français brouillon](https://sigmdel.ca/michel/ha/rpi/dnld/draft_spidev_doc_fr.pdf)<br>
>-  [In english draft](https://sigmdel.ca/michel/ha/rpi/dnld/draft_spidev_doc_en.pdf)<br>
>-  [Pypi : Python bindings for Linux SPI access through spidev](https://pypi.org/project/spidev/#description)

<br>
<details>
    <summary>CONSTRUCTEUR $spidev.SpiDev()$ & METHODES $open$, $close$ </summary><br>

>- Pour que l'**initiateur** puisse atteindre sa **cible** il est indispensable d'ouvrir le canal de communication :
>    -  soit sous la forme $CONCISE$ du constructeur avec les parametres du canal ($device$ et $cible$)
>    -  soit sous la forme $EXPLICITE$ en utilisant la méthode  $open()$ avec les parametres du canal ($device$ et $cible$)<br><br>
>- En fin de session il faux toujours fermer le canal par la méthode $close()$<br><br>
>- Une même instance peut gérer des canaux différents, mais à un instant donné elle ne peut gérer qu'un seul canal.
>  Si l'on souhaite gérer plusieurs canaux DIFFÉRENTS en même temps, il faut autant d'instances que de canaux
><br>
>
>```python
># FORME EXPLICITE
>#----------------
>import spidev<br>
>
>my_spi = spidev.SpiDev()     # En premier : Création de l'instance my_spi
>my_spi.open(device, cible)   # Ensuite    : Ouverture du canal de communication sur la cible du device
>
># votre code ici
>
>my_spi.close()               # Toujours fermer le canal en fin de traitement
>```
>
>```python
># FORME COMPACTE
>#---------------
>import spidev
>
># Création de l'instance my_spi, et ouverture du canal dans la foulée
>my_spi = spidev.SpiDev(device, cible) 
>
># votre code ici
>
> my_spi.close()             # Toujours fermer le canal en fin de traitement
>```
>
>```python
>my_spi = spidev.SpiDev() # Création de l'instance my_spi
>
>my_spi.open(d1, c1)      # ouverture du canal (d1,c1)
># votre code ici
> my_spi.close()           #fermeture du canal (d1,c1)
>
># AVEC LA MEME INSTANCE my_spi
>my_spi.open(d2, c2)      # ouverture d'un nouveau canal (d2,c2) 
># votre code ici
> my_spi.close()           # fermeture du canal (d2,c2)
>
># TOUJOURS AVEC LA MEME INSTANCE my_spi
>my_spi.open(d1, c1)      # RÉouverture du premier canal (d1,c1)
># votre code ici
> my_spi.close()           # fermeture du canal (d1,c1)          
>```
>
>```python
># GESTION DE DEUX CANAUX EN MÊME TEMPS
>#-------------------------------------
>
>my_spi_canal_A = spidev.SpiDev() 
>my_spi_canal_B = spidev.SpiDev() 
>
>my_spi_canal_A.open(d1, c1)
>my_spi_canal_B.open(d2, c2)     
># votre code ici
> my_spi_canal_A.close()           # fermeture du canal (d1,c1)
> my_spi_canal_B.close()           # fermeture du canal (d2,c2)          
>```
></details>

<details>
    <summary>GETTERs et SETTERs niveau ⭐ ⭐</summary><br>

>- **bits_per_word** : nombre de bits par mot.<br>
>   - Valeur par défaut : 8
>   - Valeurs possibles : 8 .. 16
>   - Restriction : en lecture seulement sur le Raspberry Pi. (processeur 8 bits)
>     
>- **cshigh** : indique niveau binaire de sélection du Device.<br>
>   - Valeur par défaut : $False$
>   - Valeurs possibles
>        -  $True$ Le device est sélectionné par un niveau $HAUT$
>        -  $False$ Le device est sélectionné par un niveau $BAS$
>
>- **max_vitesse_hz** :  vitesse maximale du bus SPI en Hz.
>   - Valeur par défaut : 125000000
>   - Valeurs possibles : 3800Hz à 32 MHz
>   - Remarques : Pour le Raspberry Pi<br>
>       - La valeur doit être un multiple de 2
>       - Donner une valeur raisonnable. (entre 3800 et 4800 Hz)
>       - Necessite de faire des tests de fiabilité
>
>-  **mode** : le mode SPI spécifie la polarité d'horloge et sa phase.
>    - Valeur par défaut : 0
>    - Valeurs possibles : 0, 1, 2, 3
>
>-  **lsbfirst** : lsb ::= ***Least  significant bit first*** Spécifie l'ordre d'émission des bits dans la trame série.<br>
>    - C'est un **getter**. Il est impossible de modifier ce parametre, il est en lecture seulement.
>    - Valeur par défaut : **False**<br>Donc le bit de poids fort est toujours transmis en premier
>    - Valeurs possibles : uniquement **False**
>    - Cela a une conséquence lors de la construction de la demande envoyée par le **RPi3B+** au **MB3002**.<br>
>      Ce point sera expliqué plus en détail dans le paragraphe ***Amélioration du code clé en main niveau ⭐***<br><br>
>- Il existe d'autre getter et setter mais non abordé au niveau ⭐ ⭐
</details>

<details>
    <summary>METHODES DE CLASSE niveau ⭐ ⭐</summary><br>
  
>- **close** :  Déconnecte l'objet du périphérique SPI du système.
>     -  Syntaxe : $close$()
>     -  Renvoie : rien
>
>-  **open** : Connecte l’object au périphérique SPI du système spécifié.
>     -  Syntaxe : $open$(device, cible)
>     -  Renvoie : rien
>
>- **xfer2** : Effectue une transaction SPI.
>     -   Une liste d'octets [ $Valeurs_{Initiateur}$ ] est émise, bit à bit, depuis la broche $SDO$ de l'initiateur sur la broche $SDI$ de la cible.<br>
>         En même temps, pour chaque bit reçu par la cible celle-ci emet en réponse un bit par sa broche $SDO$ sur la broche $SDI$ de l'initiateur.<br>
>         Au final, la cible aura renvoyer autant de bits que l'initiateur en aura émis.<br>
>         Donc autant d'octet, c'est à dire une liste ayant le même nombre d'éléments que la liste [ $Valeurs_{Initiateur}$ ].<br>
>         Plus concrètement :
>            -  type( $Valeurs_{Cible}$ ) = <class 'list'>
>            -  len( $Valeurs_{Cible}$ ) = len( $Valeurs_{Initiateur}$ )<br><br>
>     -  Syntaxe :
>        -  [ $Valeurs_{Cible}$ ] = $xfer2$([ $Valeurs_{Initiateur}$ ])
>        -  [ $Valeurs_{Cible}$ ] = $xfer2$([ $Valeurs_{Initiateur}$ ], $vitesse$ )
>
>     - Renvoie : [ $Valeurs_{Cible}$ ] une liste d’octets
>     - Paramètres :
>        -  [ $Valeurs_{Initiateur}$ ] : une liste d’octets
>        -  $vitesse$ : fréquence de l’horloge SPI en Hz.
>             - Si la vitesse n'est pas spécifiée, alors la fréquence d'horloge SPI est définie par l'attribut  **maximum_speed_hz**.
>             - Si la vitesse est spécifiée, elle sera utilisée uniquement le temp de cette transaction.<br>
>               Cela ne modifiera pas la valeur de l'attribut **maximum_speed_hz** de l'objet.<br><br>
> 
>- Il existe d'autres méthodes mais elle ne sont pas nécessaires au niveau ⭐ ⭐
</details> <br>
