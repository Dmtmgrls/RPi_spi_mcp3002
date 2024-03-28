### TESTS, OBSERVATIONS & ETUDE du CODE


>[!CAUTION]
> La température du boitier plastique du **TMP36** doit restée entre -40 et +125 °C.<br>
> Une bonne pratique sera de rester entre -10 et 50°C pour éviter les brulures.<br>
>
<details>
   <summary>Lancement du programme <b>step_1_read_SPI.py</b></summary><br><br>

>- Se mettre sous le réperetoire de travail puis lancer le code python "**clé en main**"<br><br>
>
>```` python
>
> cd ~/Developpement_Python/Temperature/RPi_spi_mcp3002
> python3 step_1_read_SPI.py
>````
</details>

<details>
   <summary>Résultats et observations</summary><br><br>
  
- A l'écran s'affiche les mesures de température du boitier plastique du capteur de température **TMP36**.<br>
  Ce capteur est connecté sur le **canal** 0, du **device** 0.<br>
  Si l'on ne touche à rien, les mesures de température n'évoluent pas.<br>

````
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 21.87
````
<br>

- Au bout de 10 mesures affichées (c'est dans le code) un message va apparraître.<br>
  Ce message invite à pincer entre deux doigts le boitier plastique du **TMP36** de façon à élever sa température.<br>

````
Touch the TMP36 to increase its temperature ..
````
<br>

- On constate alors qu'effectivement la température du **TMP36** augmente lentement

````
On (Device/Chanel) 0/0 T°C = 21.87
On (Device/Chanel) 0/0 T°C = 22.19
On (Device/Chanel) 0/0 T°C = 23.15
On (Device/Chanel) 0/0 T°C = 23.48
On (Device/Chanel) 0/0 T°C = 23.80
On (Device/Chanel) 0/0 T°C = 24.12
On (Device/Chanel) 0/0 T°C = 24.12
On (Device/Chanel) 0/0 T°C = 24.44
On (Device/Chanel) 0/0 T°C = 24.44
On (Device/Chanel) 0/0 T°C = 24.44
On (Device/Chanel) 0/0 T°C = 24.77
````
<br>

- A nouveau, au bout de 10 mesures (c'est dans le code) un nouveau message va apparraître.<br>
  Ce message invite à ne plus toucher le boitier plastique du **TMP36** de façon à faire baisser sa température.<br>

````
Do not touch the TMP36 so that it cools down ..
````
<br>

- On constate alors qu'effectivement la température du **TMP36** baisse lentement.

````
On (Device/Chanel) 0/0 T°C = 24.77
On (Device/Chanel) 0/0 T°C = 24.77
On (Device/Chanel) 0/0 T°C = 24.77
On (Device/Chanel) 0/0 T°C = 24.44
On (Device/Chanel) 0/0 T°C = 24.44
On (Device/Chanel) 0/0 T°C = 24.12
On (Device/Chanel) 0/0 T°C = 24.12
On (Device/Chanel) 0/0 T°C = 23.80
On (Device/Chanel) 0/0 T°C = 23.80
````
<br><br>

- Ce cycle (pincer / relâcher) se poursuit indéfiniment.<br><br>
- Pour mettre fin à ce cycle, taper sur les touches **[CTL][C]** <br>
  Un message d'erreur **KeyboardInterrupt** s'affiche, ce qui est tout à fait normal.<br><br>
- Le programme ne fini pas proprement, une correction sera apportée lors de  l'étape :star::star: du T.P.<br>
  Mais avant il faudra approfondir l'étude du principe du **bus SPI** et du package **SpiDev**

````
^CTraceback (most recent call last):
  File "/home/idca5585/Developpement_Python/Temperature/RPi_spi_mcp3002/step_1_read_SPI.py", line 46, in <module>
    time.sleep(1)
KeyboardInterrupt
````
</details>

<details>
   <summary>Etude du code de niveau :star:</summary><br>

| ℹ️ | Tout ce qui touche au bus SPI ne sera pas approfondi dans ce qui suit |
|-|-|

><details>
>    <summary>Importation des packages</summary><br>
>
>>-  Le package **spidev** est absolument indispenssable.
>>-  Le package **time** n'est utile que pour espacer les demandes de mesures de températures entre elles
>>
>> ```` python
>>import spidev
>>import time
>> ````
></details>
>
><details>
>    <summary>Les parametres ou les constantes</summary><br>
>
>>- La borne **CS** du MCP3002 a été relié à la borne **CE0** du Raspberry.<br>
>>  Le raspberry considère qu'il s'agit du **Device 0**. Ou device CE0.
>>- La broche $V_{OUT}$ du TMP36 a été relié à la borne **CH0** du MCP3002.
>>  Donc le Raspberry considère que l'entrée analogique $V_{OUT}$ du TMP36 est sur le canal 0 du device 0.
>>- La vitesse de transmission nous verons cela plus tard.
>>- Les constantes OFFSET, V_REF, SENSIBILITE seront abordées dans ce qui suit.<br>
>>```` python
>># CONST
>>CE0     = 0
>>DEVICE  = CE0
>>CHANEL  = 0
>>MAX_SPEED_HZ = 4000 # Hz
>>OFFSET       =  500 # mV
>>V_REF        = 3300 # mV
>>SENSIBILITE  =   10 # mV/°C
>>````
></details>
>
><details>
>    <summary>L'ouverture du bus SPI et le parametrage de sa vitesse de transmission</summary><br>
>
>>- Le package **sipdev** contient la classe **SpiDev** qui gère le bus SPI.
>>  Cette classe possède trois méthodes :
>>     -   l'instanciateur
>>     -   l'ouverture du bus SPI vers la cible
>>     -   le parametrge de la vitesse du bus SPI.<br>
>>- Au niveau :star:, il est inutile d'en savoir plus (pour que cela marche  il faut juste ces trois méthodes.)<br>
>>```` python
>>spi = spidev.SpiDev()            # Instanciation
>>spi.open(DEVICE, CHANEL)         # Ouversure du bus vers la cible (TMP36)
>>spi.max_speed_hz = MAX_SPEED_HZ  # Fréquence de l'Horloge de synchronisation.
>>````
></details>
>
><details>
>    <summary>La fonction <b>conversion_sample_to_degrees</b></summary>
>
>-   Cette fonction ne comporte que deux lignes de code, mais cette simplicité n'est qu'apparente.<br>
>    Pour comprendre ce code python il est nécessaire d'assimiler les trois études ci-dessous; dans l'ordre indiqué :<br>
>
>        1  [Étude fonctionnelle du TMP36](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_1/FR_STUDY_TMP36.md)<br>
>        2  [Étude fonctionnelle du MCP3002](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_1/FR_STUDY_MCP3002.md)<br>
>        3  [Étude fonctionnelle de la chaîne de mesure](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_1/FR_MEASURING_CHAIN.md)<br><br>
> 
>```` python
># Return the conversion of sampling to degrees Celsius
>def conversion_sample_to_degrees( sample ):
>    Vin_digitalised = V_REF * ( sample / ( 2 ** RESOLUTION ))
>    return (Vin_digitalised - OFFSET) / SENSIBILITE
>````
>
>><details>
>>    <summary>Explication synthétique après lecture des trois études précédentes.</summary>
>>
>>- La première ligne de code correspond à : $V_{Digitalized} = F(Code_{Numerique}) =  V_{Ref} * \frac{Code_{Numerique}}{2^N}$ en $[mV]$<br><br>
>>- La seconde ligne de code renvoie le résultat :  $\theta_{Digitalized} = f(V_{Digitalized}) = \frac{V_{Digitalized} - Offset}{Sensibility}$ en $[°C]$<br><br>
>>
>>   ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Chaine_de_Mesures.png)
>>
>></details>
><br><br>
>
></details>
>
><details>
>    <summary>La fonction <b>read_msb_lsb</b></summary><br>
>
>- Le principe du **bus SPI** n'est pas abordé au niveau :star: mais au niveau :star::star:<br><br>
>- Tout ce qu'il faut savoir c'est que :
>    -   la tension $V_{out}$ issue du capteur **TMP36** est digitalisée (codée) sous la forme d'un nombre binaire.<br>
>    -   le Raspberry lit ce nombre et le traite grâce à cette fonction pour le décoder en le mettant sous la forme d'une tension..
>
></details>
>
><details>
>    <summary>La partie <b>main</b></summary><br>
>
>>- Il faut détailler
></details>


</details>




