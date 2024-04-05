### ADRESSAGE DES CIBLES ET DU DEVICE
Niveau ⭐⭐⭐
<br><br>
<details>
  <summary>ADRESSAGE MATÉRIEL</summary><br><br>

>- L'adressage du Device est identique à l'adressage utilisé pour le montage de niveau ⭐.<br><br>
>    - La borche ***CE0*** du Raspberry est connectée à la broche $\overline{CS}$ du ***MCP3002***.
>    - La borche ***CE1*** du Raspberry n'est pas utilisée, puisque qu'il n'y a qu'un seul ***MCP3002***.<br>
>      Par conter, cette broche sera utilisée lors de l'étude du niveau ⭐⭐⭐⭐
><br><br>  
</details>

<details>
  <summary>ADRESSAGE LOGICIEL AU NIVEAU DU PACKAGE $spidev$</summary><br><br>

>- La méthode $open($ code_device, code_chanel $)$ :
>
>  |code python |  dialogue avec le MCP3002 avec sa broche $\overline{CS}$ $\leftarrow$ ***CE0*** |
>  |---|---|
>  |   open(0,0)  | le canal ***CH0*** 
>  |   open(0,1)  | Le canal ***CH1***  

<br><br>

>- La méthode $V_{Digitalisation}=xfer2($ listeDemande, vitesse $)$ :
>
>  | Canal |$Start$ | $Sgl$  | $Odd$  | $Msbf$ |  Position<br>0b0***SSO.M***000 | ***Mot MSB*** | ***Mot LSB***  | $listeDemande$ |
>  |---|---|---|---|---|---|---|---|---|
>  | ***CH0*** |  ***1***  | ***1*** | ***0*** | ***1*** | 0b0***110.1***000 | $0x68$ | $0x00$ | [ $0x68$, $0x00$ ]  |
>  | ***CH1*** |  ***1***  | ***1*** | ***1***  | ***1*** | 0b0***111.1***000 | $0x78$ | $0x00$ | [ $0x78$, $0x00$ ]  |
</details>

