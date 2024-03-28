### AMELIORATION DU CODE :star::star:<br><br>


<details>
<summary>ETUDE PLUS PRÉCISE DU $MCP3002$</summary>
<br>
  
Ce qui suit provient de la page https://www.microchip.com/en-us/product/MCP3002#document-table<br>
à partir de laquelle faut télécharger le document :<br>
**MCP3002 - 2.7V Dual Channel 10-Bit A/D Converter with SPI Serial Interface 	Data Sheets 	DS21294** 

><details>
>    <summary>ETUDE DU CHRONOGRAMME DU $MCP3002$ DU CONSTRUCTEUR</summary><br>
>
>- Le shéma est issu de la page 15 du document pré-cité. Il a été modifié pour être compris au niveau :star::star:<br><br>
>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_Chronogramme_niveau_2.png)<br><br>
>
> $ATTENTION$ Pour que le Raspberry puisse obtenir la valeur de la température digitalisée par le MCP3002, le Raspberry doit<br>
>respecter ce chronogramme, et les informations qu'attend le MCP3002.<br>
>- A droite en rouge la cible $MCP3002$ avec ses 4 broches  $\overline{CS}$, $SLCK$, $SDI$, et $SDO$
>- A gauche en vert  l'initiateur $RPi_{3B+}$ avec ses  4 broches  $\overline{CS}$, $SLCK$, $SDI$, et $SDO$.<br>
>  C'est l'initiateur qui a la maîtrise de $\overline{CS}$ et $SLCK$.<br><br>
>- De haut en bas au centre nous avons la forme des signaux sur les broches $\overline{CS}$, $SLCK$, $SDI$, et $SDO$<br>
>  Le temps s'écoule de gauche vers la droite.<br><br>
>   - $\overline{CS}$ est actif au niveau bas. L'initiateur sélectionne la cible par un niveau bas sur sa broche $\overline{CS}$<br>
>   - $SLCK$ est fourni par l'initiateur, et son évolution temporelle ne dépend que de lui.<br>
>   - $SDI$ de la cible reçois de la part de l'initiateur sa demande (bits dans le restangle bleu).<br>
>   - $SDI$ de l'initiateur reçois la réponse de la part de la cible (bits dans le rectangle orange).<br><br>
>- Ensuite nous avons la représentation des 16 bits échangés entre la cible et l'initiateur :<br><br>
>   - Dans le rectangle bleu sont représentés les 4 bits indispensables qu'attend le MCP3002 de la part de l'initiateur.<br>
>     Ils signifient : **Donne moi la température digitalisée**<br>
>   - Dans le rectangle orange sont représentés la position des 10 bits correspondant à la **Valeur digitalisée de la température**<br><br>
>
>- $Petite$ $explication$<br>
>    - La valeur digitalisée de la température est sur 10 bits. Or le bus SPI n'échange que des *octets*.<br>
>      C'est ce que nous indique le chronogramme constructeur en indiquant un échange de 16 bits sous forme de deux mots de 8 bits.
>      Par conséquent, l'initiateur doit émettre deux octets, soit 16 bits pour ne considérer que 10 d'entre eux.<br><br>
>
>    - Lors de l'étude du code $CléEnMain$ du niveau :star: la fonction qui récupère la valeur de la température n'a pas été présenté.<br>
>      Lors de son étude nous verrons que trois octets ont été utilisés. **Pourquoi?** C'est le sujet du correctif présenté plus loin.
></details>
>
>
><details>
>    <summary>ETUDE DE LA DEMANDE ATTENDUE PAR LE $MCP3002$ </summary><br>
>
>><details>
>>    <summary>Résumons ce qui se passe dans une transaction.</summary><br>
>>
>>
>>-  Résumons ce qui se passe dans une transaction.<br>
>>   L'Initiateur (**RPi3B+**) va :
>>    - Sélectionner la cible (**MCP3002**) en activant sa broche $\overline{CS}$ au niveau binaire $BAS$.<br>
>>    - Transmettre deux octets qui servirons :<br>
>>       - Dans le sens  **Initiateur** $\to$ cible :
>>           - Le premier octet contiendra les informations attendues par la cible.
>>           - Le second octet est indispensable mais n'est pas utilisé par la cible.<br>
>>       - Dans le sens  **cible**  $\to$ **Initiateur** :
>>           - Le premier octet contiendra les  deux bits de poids fort des 10 bits de la digitalisation de la température.
>>           - Le second octet contiendra les huits bits de poids restants des 10 bits de la digitalisation de la température.
>>
>></details>
>>
>><details>
>>    <summary>Que contient le premier octet dans le sens <b>Initiateur</b> &#8594; <b>cible</b> ?</summary><br>
>>
>>    - Il contient 4 bits d'information. A chacun de ces bit est associé un nom décrivant sa fonction :<br>
>>       ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_Chronogramme_Start_SGL_Odd_MSBF_niveau_2.png)<br><br>
>>
>>       -  $START$ Ce bit indique que les informations ***SGL***, ***ODD***, et ***MSBF*** suivent.<br>
>>          Ce bit sera toujours à l'état binaire $1$<br><br>
>>
>>       -   $SGL$ ce bit se nomme en réalité ***SGL/DIFF***, mais pour ce T.P. c'est le mode ***SGL*** qui est choisi.<br>
>>           Il signifie ***single ended mode*** , c'est à dire ***mode signal asymétrique***.<br> 
>>           Ce bit sera toujours à l'état binaire $1$.<br><br>
>>
>>        - $ODD$ ce bit se nomme en réalité ***ODD/SIGN***, mais pour ce T.P. c'est le mode ***ODD*** qui est choisi.:<br>
>>          Sa fonction est de sélectionner le canal de la cible :<br> 
>>              -  ***ODD*** = $0$, le canal $0$ est sélectionné.<br>
>>              -  ***ODD*** = $1$, le canal $1$ est sélectionné.<br><br>
>>
>>        - $MSBF$ ce bit indique à la cible comment la valeur digitalisée doit être transmise :<br>
>>             - ***MSBF*** = $1$ : Le bit de poids **fort** sera transmis en premier.<br>
>>               Pour ce T.P. ce bit sera toujours à l'état binaire $1$<br>
>>             - ***MSBF*** = $0$ : Le bit de poids **faible** sera transmis en premier.<br><br>
>>
>></details>
>>
>><details>
>>    <summary>Où sont placés ces 4 bits parmis les 16 bits qui vont être échangés ?</summary><br>
>>
>>- Le constructeur l'indique dans son chronogramme : <br>
>>
>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_Chronogramme_Ask_Position_niveau_2.png)
>>
>>- La liste d'octets que doit émettre l'initiateur sont de deux formes possible selon la cible choisie :<br> 
>>
>> | cible | Octet de poid fort | octet de poid faible | l'initiateur doit émettre<br>cette liste d'octets| 
>> |----|----|----|----|
>> | ***ODD*** | 0b0***Start-SGL-ODD-.MSBF***000 | 0b0000.0000 |  |
>> | O | 0b0***110.1***000 | 0b0000.0000 | [ 0x68, 0x00 ] |
>> | 1 | 0b0***111.1***000 | 0b0000.0000 | [ 0x78, 0x00 ] |
>>  
>></details>
>
></details>
>
><details>
>    <summary>ETUDE DE LA RÉPONSE ATTENDUE PAR LE $RPi{3B+}$</summary><br>
>
>- Le constructeur l'explique à la page 15 du document pré-cité en introduction.<br>
>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES//MCP3002_Chronogramme_Reponse_niveau_2.png)<br><br>
>
>-  L'Initiateur recevra deux octets :<br>
>    - L'un contiendra les deux bits de poids fort $B_{9}$ et $B_{8}$ de la tension digitalisée sur 10 bits<br>
>      $B_{9}$ sera précédé par un bit = $0$<br><br>
>    - L'autre octet contiendra les 8 bits $B_{7}$ à $B_{0}$ restants de la tension digitalisée sur 10 bits.<br>
>
></details>
</details>

<details>
<summary>L'EFFET BOITE NOIRE DU PACKAGE $spidev$ </summary>
<br>
  
>- Le package $spidev$ masque le mécanisme de gestion des broches $\overline{CS}$, $SLCK$, $SDI$, et $SDO$.<br>
>  Mais nous pouvons mieux comprendre que ce que font les deux méthodes $open$ et $xfer2$<br><br>
>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_Black_Box_spidev_niveau_2.png)<br><br>
>
>- le parametre $device$ de la méthode $open$ permet d'activer la broche $\overline{CS}$<br><br>
>
>- le paramettre $vitesse$ de ma métode $xfer2$ permet de fixer la bonne fréquence de l'horloge de synchronisation.<br>
>- La liste verte  $[MSB, LSB]$ en émission permet de demander la $V\theta_{Digitalisee}$ à la cible.<br>
>- La liste rouge $[MSB, LSB]$  en réception contient $V\theta_{Digitalisee}$.<br>
>     - Cette liste en rouge est le résultat renvoyé par la méthode $xfer2$.<br>
>     - Cette liste en rouge à la même taille que la liste en vert émise pour faire la demande.<br>
>     - l'élément $0$ de la liste en rouge contient les deux bits MSB de $V\theta_{Digitalisee}$.<br>
>     - l'élément $1$ de la liste en rouge contient les huit bits LSB de $V\theta_{Digitalisee}$.<br><br>
>
>- Le calcule de la valeur de $V\theta_{Digitalisee}$ en langage python s'écrira en développant chaque étape du calcul :<br>
>
>````python
>My_spi = spidev.SpiDev()
>
># Demande de la tension, et réception de celle-ci dans la variable liste_recue
>liste_recue = My_spi.xfer2( liste_demande, vitesse)
>
># Récupération des deux bits de poids faible dans l'octet de poids fort par un ET binaire.
>octet_b9b8 = Liste_recue[0] & 0x03
>
># Decalage à gauche de 8 bits pour que les deux bits passent en positon B9,B8 pour un nombre de 16 bits
>msb_16 = octet_b9b8 << 8
>
># Récupération de l'octet de poids faible
>lsb_8 = liste_recue[1]
>
># Calcul de la valeur digitalisee qui est un nombre sur 16 bits 0, 0, 0, 0, 0, 0, B9, B8, B7,..., B1, B0
>valeur_digitalisee = msb_16 + lsb_8
>
># Sous la forme condensée le calcul se résume ainsi
>valeur_digitalisee = ( liste_recue[0] & 0x03 ) << 8 + liste_recue[1]
>````
</details>

<details>
    <summary>ETUDE DE LA FONCTION $CléEnMain$ et amélioration du code python</summary></summary>
<br>
 
>- Voici le code $CléEnMain$ de niveau :star: nous allons détailler chaques lignes.<br>
>
>```` python
># Return the digitalized value of the input voltage on MCP3002
>def read_msb_lsb():
>    data = spi.xfer2([ 1, 2<<6, 0])  # ligne N°1
>    msbyt = (data[1] & 0x0F) << 6    # ligne N°2 
>    lsbyt = (data[2] >> 2)           # ligne N 3
>    return msbyt  | lsbyt            # ligne N°4
>````
>
>- Et voici le chronogramme correspondant.<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Code_Avant_niveau_2.png)<br><br>
>
>- $Ligne_{N°1}$ appel de la méthode ***xfer2*** de l'instance $spi$ de la classe **spidev.SpiDev**.<br>
>    - Son parametre est une liste de **trois** octets $[ 1, 2<<6, 0 ]$ :<br><br>
>
>     | octet |  Valeur | Signification |
>     |---|---|---|
>     | 1 | $1 = 0x01$ |  $START = 1$ |
>     | 2 | $2 = 0x02$<br> avec un décalage de 6 bits vers la gauche soit $0x80$ |  Cela corresponds aux bits<br> $SGL=1$ , $ODD=0$ , et $MSBF=0$<br> **MSBF** = 0 au lieu de **1** !!!|
>     |3 | $0x00$ | Il ne porte aucune information.<br>|
>
>   -  Le résultat de la conversion Analogique/Numérique se trouve dans la variable $data$ qui est une liste de trois octets.<br><br>
>
>
>- $Ligne_{N°2}$ Calcul à partir de data[1] qui représente l'octet $2$ de la réponse.
>    - Tout d'abord on fait un ET binaire avec le masque $0x0F$. Ce qui donne ***0000.B9, B8, B7, B6***
>    - Ensuite, un décalage à gauche de 6 bits est appliqué à ce résultat. Le nouveau résultat est un mot de 16 bits.<br>
>      C'est à dire que  le mot de 8 bits ***[B9][B8].[B7][B6].0000*** devient un mot de 16 bits ***0000.00[B9][B8].[B7][B6]00.0000.0000***<br><br>
>
>- $Ligne_{N°3}$ Calcul à partir de data[2] qui représente l'octet $3$ de la réponse.
>    - Là on décale de deux bits vers la droite pour positionner le bite $B0$ sur le bit de poids faible.<br>
>      C'est à dire que le mot de 8bits [B5][B4][B3][B2].[B1][B0]00 devient 00[B5][B4].[B3][B2][B1][B0]<br>
>
>- $Ligne_{N°4}$ Renvoie le $OU$ binaire de  $MSB$ et $LSB$<br><br>
>    - On obtient bien un mot de 16 bits dont les 10 premiers bits correspondent à la tension digitalisée issue de l'entrée du C.A.N.<br><br>
>
>  | A | A | A | A | A |
>  |---|---|---|---|---|
>  | MSB | 0 0 0 0 | 0 0 [B9][B8] | [B7][B6] 0 0 | 0 0 0 0 |
>  | LSB |         |              | 0 0 [B5][B4] | [B3][B2][B1][B0] |     
>  |  =  | 0 0 0 0 | 0 0 [B9][B8] | [B7][B6][B5][B4] | [B3][B2][B1][B0] |
>  <br>
>
</details>

<details>
    <summary>COMPARAISON, OBSERVATIONS, ET EXPLICATIONS</summary></summary><br>
  
><details>
>    <summary>Comparaisons des deux codes</summary></summary><br>
>  
>>- Le code de niveau :star: utilise trois octets, et le calcul de la tension digitalisée n'est pas évidente.<br><br>
>>
>>````
>>def read_Vdigitalized_level_1_on_chanel_0():
>>   # 0xD0 :: 0b1100.0000
>>   #           SDCM.oooo
>>   ADC = spi.xfer2([0x01,0xD0, 0x00])
>>   return ((ADC[1] & 0x0F) <<6) | ADC[2] >> 2 
>>
>>````
>><br>
>>
>>-  Le code de niveau :star::star: n'utilise que deux octets, et le calcul de la tension digitalisée est évident.<br>
>>   C'est ce code qui sera utilisé par la suite.<br><br>
>>
>>````
>>def read_Vdigitalized_level_2_on_chanel_0():
>>   # 0x60 :: 0b0110.0000
>>   #           oSDC.Mooo
>>   ADC = spi.xfer2([0x60, 0x00])
>>   return (ADC[0]<<8) | ADC[1]
>>````
>>
>>
></details> 
>
><details>
>    <summary>Observation concernant la valeur de MSBF</summary></summary><br>
>  
>>-  Le code simplifié donne le même résultat quelque soit la valeur du bit ***MSBF***<br>
>>   Sur le code $CléEnMain$ niveau :star: ce comportement s'observe également.
>>
>>  - Ci-dessous un programme de test, où la demande se fait une fois avec $MSBF = 0$, et une fois $MSBF = 1$
>>
>>````python
>>import spidev
>>import time
>>
>>spi = spidev.SpiDev()
>>spi.open(0,0)           # CE0 -> CS
>>
>>#The Parameter ssom is a byte which contains the 4 bits START / SLC / ODD / MSBF
>># ssom is an acronym of Start / Slc / Odd / MsbF
>>def get_digitalized_temp(ssom):
>>   ADC = spi.xfer2([ssom, 0x00], 4000)
>>   mV = (ADC[0]<<8 | ADC[1]) * 3300.0 / 1024.0  # convert to mV
>>   return (mV - 500.0) / 10.0                   # temperature in °C
>># ----------------------------------------------------------------------
>>
>>while True:
>>   # ssom = 0 S S O . M 0 0 0 c'est à dire  0 1 1 0 . 0 0 0 0 
>>   temp = get_digitalized_temp( 0x60)
>>   print("MSBF = 0, Temperature = %5.2f " %temp)   # display temperature for MSBF = 0
>>
>>   # ssom = 0 S S O . M 0 0 0 c'est à dire  0 1 1 0 . 1 0 0 0 
>>   temp = get_digitalized_temp( 0x68)
>>   print("MSBF = 1, Temperature = %5.2f \n" %temp) # display temperature for MSBF = 1
>>   time.sleep(1)  
>>````
>><br>
>>
>>-  Le résultat du test ci-dessous montre bien que la valeur du bit $MSBF$ n'a aucune influence.<br>
>>
>>```` pyton
>>MSBF = 0, Temperature = 22.19
>>MSBF = 1, Temperature = 22.19
>>
>>MSBF = 0, Temperature = 22.19
>>MSBF = 1, Temperature = 22.19
>>
>>MSBF = 0, Temperature = 22.19
>>MSBF = 1, Temperature = 22.19
>>````
></details> 
>
><details>
>    <summary>Explications</summary></summary><br>
>  
>>-  Lors de l'étude du ***package sipdev***, il a été fait mention de l'existence d'un getter nommé $lsbfirst$<br>
>>   Sa valeur revoie toujours la valeur ***False***, et ne peut être modifié.<br>
>>   Ce qui signifie que les octets sont toujours transmis avec le bit ***MSBF = 1***<br>
>>
>>-  N'ayant pas le code du ***package sipdev*** il n'est pas possible de savoir si la demande qui est transmise à la cible<br>
>>   est systématiquement corrigée avec le bit $MSBF = 1$<br>
>>
>>- Ce n'est qu'une hypotèse qui sera confirmée ou infirmée en ayant le code du package en main.<br>
>>
></details> 
</details>  


