### ETUDE DU Bus S.P.I. :star:


> [!IMPORTANT] 
> Dans ce document nous utiliserons les termes **Initiateur / Cible** à la place de **Maître / Esclave**.
>    - Ces termes sont plus appropriés par rapport au fonctionnement du Bus S.P.I.<br>
>    - [Mais aussi pour cette raison : ](https://www.courrierinternational.com/article/informatique-github-veut-abandonner-les-termes-maitre-et-esclave)

> [!NOTE]
>   Ce qui suit est un résumé d'un document sur le Web.<br>
>   - [En français](https://fr.wikipedia.org/wiki/Serial_Peripheral_Interface)<br>
>   - [In english](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface)
<br>
<details>
  <summary>VOCABULAIRE & DEFINITIONS.</summary><br>

>- Définitions :<br>
>    -  Le terme **SPI** signifie **S**erial **P**eripheral **I**nterface.<br>
>    -  C'est est un bus de données série **synchrone**.
>    -  Par rapport à un bus par **contrôle de flux** (Ex. I2C), le bus SPI est très simple, très rapide, et  sans risque de colision.<br>
>       Cependant cette simplicité rend impossible l'assurance que les données ne sont pas perdues.<br><br>
>- Structure physique du Bus :
>    - Le $Bus$ comporte les connexions suivantes .<br>
>         - Deux connexions $Bus$ pour la transmission de données : un pour la **Demande** un pour la **Réponse**.<br>
>         - Une connexion $Bus$ de synchronisation (ou encore **Horloge**) géré uniquement par l'initiateur.<br>
>         - Autant de connexion de $Sélection$ que de **Device** avec lesquels l'initiateur veut communiquer.<br>
>           Ce qui est un incovénient qui complique le cablâge ou  le design des circuit imprimés.<br><br>
>
>- Terminologie adoptée dans ce document concernant les broches des circuits électroniques connectés au bus SPI.<br><br>
>
>    |  Fonction | Broche<br> Initiateur |  | direction | | Broche<br>  Cible |
>    |---|---|---|---|---|---|
>    | Horloge | $SCLK$ | OUT | $\to$ | IN | $SCLK$ |
>    | Demande initiateur | $SDO$ | OUT |  $\to$ | IN | $SDI$ |
>    | Réponse cible | $SDI$ | IN | $\gets$ | OUT | $SDO$ |
>    | Sélection Device 1 | $CS_{1}$| OUT| $\to$| IN|  $CS$ $Device_{1}$ |
>    | Sélection Device j | $CS_{j}$| OUT | $\to$| IN |  $CS$ $Device_{j}$ |
>    | Sélection Device n| $CS_{n}$| OUT | $\to$| IN |  $CS$ $Device_{n}$ |
>
>    $SDO = SerialDataOut$ ;  $SDI = SerialDataIn$ ; $SCLK = SerialClock$ ; $CS = ChipSelct$
</details>
<br>
<details>
  <summary>CARACTERISTIQUES DE L'HORLOGE (CLOCK).</summary><br>

>- La gestion de l'horloge est sous la responsabilité de l'initiateur de la communication (dans notre cas c'est le Raspberry)
>- A chaque coup d'horloge il y a :
>    - Émission d'un **bit de demande** depuis l'initateur vers la cible.
>    - Émission d'un **bit de réponse** depuis la cible vers l'initiateur.
>    - Nous considèrerons au niveau :star::star: que les deux émissions sont "simultanées".<br>
>      Elles se poursuivent jusqu'à ce que l'horloge soit placée dans l'état $Inactif$ par l'initiateur.<br><br>
>
>- Deux nouveaux termes sont apparrus dans cette description : $Coup$ $d'Horloge$ et $État$ $inactif$ $Horloge$<br>
> 
>    - Qu'entend on par $Coup$ $d'Horloge$ ? Est-ce le front montant $\nearrow$ ou est-ce le front descendant $\searrow$ du signal d'Horloge ?<br>
>      Par la suite ce front sera nommé $CLOK_{Phase}$ ou $C_{PHA}$<br><br> 
>    - Qu'entend t'on par $État$ $inactif$ $Horloge$ ? Est-ce l'État binaire Haut ou Est-ce l'État binaire bas ?<br>
>      Par la suite cet État binaire sera nommé : $CLOCK_{Polarité}$ ou $C_{POL}$.<br><br>
>
>- Ces 4 possibilités sont appelées $Mode$, où $C_{POL}$ et $C_{PHA}$ sont des bits de configuration.<br>
>
> | $Mode$  |   $C_{POL}$  | $C_{PHA}$    | $Front$ |  Horloge<br> État binnaire<br> Inactif (Idle) | $Commentaire$ |
> |----|----|----|----|----|----|
> |0| 0 | 0 | $\nearrow$| $Bas$ | L'initiateur lit la donnée sur sa broche $SDI$ sur $\nearrow$ |
> |1| 0 | 1 | $\searrow$ | $Bas$ | L'initiateur lit la donnée sur sa broche $SDI$ sur  $\searrow$<br> C'est le $Mode$ le plus couramment adopté |
> |2| 1 | 0 | $\searrow$ | $Haut$ |  L'initiateur lit la donnée sur sa broche $SDI$ sur  $\searrow$  |
> |3| 1 | 1 | $\nearrow$ |$Haut$ | L'initiateur lit la donnée sur sa broche $SDI$ sur $\nearrow$  |  
><br>
> $Remarque$ : Ce n'est pas $C_{PHA}$ seul qui représente le front mais le couple ($C_{POL}$ ; $C_{PHA}$)
>
>    -  Si $C_{POL}$ est **BAS** le frond actif ne peut pas être $\searrow$ mais $\nearrow$.
>    -  Inversement, si $C_{POL}$ est **HAUT** le frond actif ne peut être $\nearrow$ mais $\searrow$.<br><br><br>
>
>$Chronogramme$
>- Ci-dessous Le chronogramme simplifié du $Mode1$. Ce chronogramme correspond au niveau  :star::star:<br>
>  Les Bits de donnés placées par la **cible** sur la broche $SDI$ de l'initiateur, sont lues par l'initiateur sur $\searrow$ de l'Horloge.<br><br>
>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_chronogramme.png)
>
</details>




