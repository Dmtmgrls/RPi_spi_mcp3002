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
>
>- Au niveau de Linux, nous avons dans le répertoir **\dev** les deux accès $CH_{O,1}$ correspondant au SPI associé au ***CE0***.<br>
>
>````python
>ls -l /dev/*spi*
>
>crw-rw---- 1 root spi 153, 0 Apr  5 22:17 /dev/spidev0.0   <-- CE0, CH0
>crw-rw---- 1 root spi 153, 1 Apr  5 22:17 /dev/spidev0.1   <-- CE0, CH1
>````
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
>  | Mode | Canal |$Start$ | $Sgl$  | $Odd$  | $Msbf$ |  Position<br>0b0***SSO.M***000 | ***Mot MSB*** | ***Mot LSB***  | $listeDemande$ |
>  |---|---|---|---|---|---|---|---|---|---|
>  | Asymétrique | ***CH0*** |  ***1***  | ***1*** | ***0*** | ***1*** | 0b0***110.1***000 | $0x68$ | $0x00$ | [ $0x68$, $0x00$ ]  |
>  | Asymétrique | ***CH1*** |  ***1***  | ***1*** | ***1***  | ***1*** | 0b0***111.1***000 | $0x78$ | $0x00$ | [ $0x78$, $0x00$ ]  |
</details>

