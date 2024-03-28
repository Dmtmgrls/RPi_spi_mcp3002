###  COMMUNICATION LORS DU TRANSFERT DE DONNÉES SUR LE BUS SPI ⭐⭐

<br><br>

<details>
  <summary>DESCRIPTION DU MECANISME</summary>
<br>

>-  Le transfert de données n'est ni une écriture seule, ni une lecture seule, mais un simultanéité d'échange de bits.
><br>
>
>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_Initial.png)
>
>- A gauche est shématisé l'**Initiateur**. Lui seul possède et gère une **horloge de synchronisation** ( $SCLK$ ).
>- A droite est shématisé la **cible** qui se synchronise sur l'horloge.
>- Des deux côté nous avons
>    - Une mémoire qui peut écrire $W$ ou lire $R$ son propre registre à décalage
>    - Un registre à décalage dont la fonction est  :<br>
>        -1   **Émettre** son bit de poid fort en ecrivant ce bit sur la broche $SDO$.<br>
>        -2   **Décaler** tous ses bits dans la direction poid faible :arrow_right:  poids forts; libérant ainsi la place du poid faible.<br>
>        -3   **Lire** le bit présent sur la broche $SDI$ et mémoriser ce bit dans le poid faible (libre en raison du décalage précédant).<br>
>    - Une broche de sortie $SDO$ (Serial Data Out).
>    - Une broche d'entrée  $SDI$ (Serial Data In).
</details>

<details>
  <summary>ECRITURE DE L'OCTET A TRANFÉRER DANS LES REGISTRES</summary>
<br>

>- L'horloge ordonne les actions suivantes selon un mécanisme qui ne nous intéresse pas au niveau :star::star:<br>
>    -  l'initiateur écrit $W$ dans son registre à décalage l'octet en mémoire qu'il souhaite transférer à sa cible.<br>
>    -  La cible fait de même de son côté avec l'octet que lui a demandé l'initiateur.<br><br>
>- Dans ce schéma très simplifié :
>    -  La couleur des bits permet de repérer leur position au fils des ordres de l'horloge de syncronisation.<br>
>       Ainsi, côté initiateur les bits sont dans les tons bleus , alors que côté cible les bits sont dans les tons ocres.<br>
>    -  De même, les bits de poids fort sont de couleur sombre, alors que les bits de poids faible sont de couleur claire.<br><br>
>
>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_W.png)
>
</details>


<details>
  <summary>TRANSFERT DES OCTETS BIT A BIT</summary>
<br>
  
><details>
>  <summary>TRANSFERT DU PREMIER BIT</summary>
><br>
>
>>
>>- L'horloge ordonne les actions suivantes :<br>
>>    -1  l'initiateur et la cible doivent **simultanément** placer le bit de poid fors de leur registre  à décalage sur la broche $SDO$.<br>
>>    -2  l'initiateur et la cible doivent **simultanément** décaler leur registre à décalage d'un bit : poids faible :arrow_right: poids fort.<br>
>>    -3  l'initiateur et la cible doivent **simultanément** lire leur broche $SDI$ et placer le bit lu dans le registre à décalge en poids faible.<br><br>
>>- Résultat :
>>   - Le bit bleu sombre de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [0]
>>   - Le bit ocre sombre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [0] <br><br> 
>>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_1.png)
>>
></details>
>
><details>
>  <summary>TRANSFERT DU SECOND BIT</summary>
><br>
>
>>- L'horloge ordonne les actions suivantes qui sont identiques à celles du transfert du premier bit:<br>
>>    -1  l'initiateur et la cible doivent **simultanément** placer le bit de poid fors de leur registre  à décalage sur la broche $SDO$.<br>
>>    -2  l'initiateur et la cible doivent **simultanément** décaler leur registre à décalage d'un bit : poids faible :arrow_right: poids fort.<br>
>>    -3  l'initiateur et la cible doivent **simultanément** lire leur broche $SDI$ et placer le bit lu dans le registre à décalge en poids faible.<br><br>
>>- Résultat :
>>   - Le bit bleu de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [0]<br>   
>>   - Le bit bleu sombre de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [1]<br><br> 
>>   - Le bit ocre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [0] <br>  
>>   - Le bit ocre sombre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [1] <br><br>
>>
>>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_2.png)
>>
></details>
>
><details>
>  <summary>TRANSFERT DU SEPTIEME BIT</summary>
><br>
>
>>- L'horloge ordonne toujours les trois mêmes actions que nous ne décrivons plus.<br><br>
>>- Résultat :
>>   - Le bit bleu de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [5]<br>   
>>   - Le bit bleu sombre de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [6]
>>   - le bit vert de l'initiateur est en position [7] **poids fort** dans le registre à décalage de l'initiateur.<br><br> 
>>   - Le bit ocre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [5] <br>  
>>   - Le bit ocre sombre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [6]
>>   - le bit saumon de la cible est en position [7] **poids fort** dans le registre à décalage de la cible.<br><br> 
>>
>>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_7.png)
>>
></details>
>
><details>
>  <summary>TRANSFERT DU HUTIEME BIT</summary>
><br>
>
>>- L'horloge ordonne **pour la dernière fois** les trois mêmes actions que nous ne décrivons plus.<br><br>
>>- Résultat :
>>   - Le bit bleu de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [6]<br>   
>>   - Le bit bleu sombre de l'initiateur se trouve à présent dans le registre à décalage de la cible au poids [7]
>>   - le bit vert de l'initiateur est en position [0] **poids faible** dans le registre à décalage de la cible.
>>   - Le tranfert de l'octet de l'initiateur a été transmis à la cible.<br><br> 
>>  
>>   - Le bit ocre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [6] <br>  
>>   - Le bit ocre sombre de la cible se trouve à présent dans le registre à décalage de l'initiateur au poids [7]
>>   - le bit saumon de la cible est en position [0] **poids faible** dans le registre à décalage de l'initaiteur.
>>   - Le tranfert de l'octet de la cible a été transmis à l'initiateur .<br><br> 
>>
>>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_8.png)
>>
</details>
  
<details>
  <summary>LECTURE DES REGISTRES ET MEMORISATION</summary>
<br>

>- L'horloge ordonne les actions : <br>
>    -  l'initiateur lit $R$ son registre à décalage et place l'octet trransmis par la cible en mémoire.<br>
>    -  La cible fait de même de son côté avec l'octet que lui a transmis l'initiateur.<br><br>
>
>  ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/SPI_Xfert_R.png)
</details>


