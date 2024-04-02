### LES NOUVEAUX PROGRAMMES
Niveau ⭐⭐⭐
<br><br>
<details>
   <summary>ETUDE DU MONTAGE AVEC $CH0$ RELIÉ à $CH1$ SUR LA MÊME SORTIE DU TMP36</summary><br>

><details>
>  <summary>But de ce montage</summary><br>
>
>>- Ce montage va permettre d'observer le comportement des deux canaux en leur appliquant le même signal à leur entrée.<br><br>
>>    - Est-ce que la tension digitalisée est identique pour les deux canaux?<br><br>
>>    - S'il y a un écart entre les mesures, comment évolue cet écart au fils du temps?<br><br>
>>    - Lors de l'usage de la méthode $xfer2(List,$ $vitesse)$ que se passe t'il si à chaque mesure :
>>        - On maintient le paramettre $vitesse$ constant sur le canal ***CH0*** (ex. 4000Hz)
>>        - On modifie le paramettre $vitesse$ sur le canal ***CH1*** (ex. 8000Hz, 10.000Hz, 50.000Hz )
></details>

><details>
>  <summary>Les programmes de test</summary><br>
>
>><details>
>>   <summary>Observation de l'écart de mesure pour un même signal d'entrée à la vitesse constante de <b>4000Hz</b></summary><br>
>> 
>>><details>
>>>   <summary>Ce que fait le code</summary><br>
>>>
>>>- Toutes les 10 milisecondes une mesure est réalisée sur le canal ***CH0***, suivie d'une mesure sur le canal ***CH1***.<br>
>>>  Cet enchaînement s'appelle un ***couple de mesures***.
>>>- A chaque couple de mesures le compteur ***nb_mesures*** est incrémenté.
>>>- Les deux mesures du couple sont comparées :
>>>    - Si les deux mesures sont identiques rien n'est affiché.
>>>    - Si les deux mesures sont différentes en + ou en -, alors une ligne d'information est affichée.<br>
>>>      Le compteur nb_gap qui compte les couple distincts est incrémenté.<br><br>
>>>- Pour sortir du programme il faut taper $[Ctrl][c]$
>>><br><br>
>>>
>>>````
>>> CONTENU DE LA LIGNE D'INFORMATION 
>>>
>>> nb_measures = 4404    nb_gap =  32   Tch0 - Tch1 = -0.3223  p = 0.727 %
>>>
>>> où
>>>
>>> nb_measures      : c'est le nombre total (4404) de couples de mesures depuis le lancement du test.
>>> nb_gap           : c'est le nombre total (32) de couples de mesures distinctes depuis le lancement du test.
>>> Tch0 - TCH1      : c'est la différence de température (-0.3223 °C) entre les mesures d'un même couple.
>>> p en pourcentage : c'est la valeur (0.727 %)  du ratio = (nb_gap/nb_measures) x 100 en %.
>>>````
>>>
>>>
>>>- On remarque que :
>>>   - Les écarts de température digitalisée sont négatifs ou positifs (  nb_gap = 8 )
>>>   - les valeurs absolues des écarts sont égales. ($0.3223$)
>>>   - le ratio de températures digitalisées distinctes est très faible. ($<1$***%***)
>>>   - L'apparition d'un couple distinct semble alléatoire : <br>
>>>     Le temps séparant deux couple distincts n'est pas constants (  $\approx10ms * nb_{measures}$)
>>><br><br>
>>>
>>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/level3_ch0ch1_connected_test_1.png)<br><br> 
>>></details>
>>>
>>><details>
>>>   <summary>Le code</summary><br>
>>>
>>>````python
>>>import spidev
>>>import time
>>>
>>>#Constants & parameters
>>>CE0       = 0  #  CE0  of  RPiB3+ connected on  CS of MCP3002
>>>SPI_BUS_0 = 0  #  system device either /dev/spidev0.0  or  /dev/spidev0.1
>>>
>>>SPEED_STANDAR = 4000    # Hz
>>>REQUEST_CH0 = [ 0x60, 0x00 ] # query to obtain the digitalization voltage on CH0
>>>REQUEST_CH1 = [ 0x70, 0x00 ] # query to obtain the digitalization voltage on CH0
>>>
>>>RESOLUTION    =  10             # bits
>>>NB_OF_SAMPLES = 2**RESOLUTION
>>>OFFSET        =  500.0          # mV
>>>V_REF         = 3300.0          # mV
>>>SENSIBILITE   =   10.0          # mV/°C
>>>
>>># Create instance
>>>spi = spidev.SpiDev()
>>>
>>># Open /dev/spidev0.0  with   CE0 -> CS of MCP3002
>>>spi.open( SPI_BUS_0, CE0 )
>>>
>>>#  Return the digitalized temperature directly
>>>def get_digit_temp( request, speed ):
>>>   digitalValue = spi.xfer2( request , speed)
>>>   digitalTension = (digitalValue[0] <<8 | digitalValue[1])  * V_REF / NB_OF_SAMPLES
>>>   digitalTemperature = (digitalTension - OFFSET) / SENSIBILITE
>>>   return digitalTemperature
>>>
>>># Main
>>>#------
>>>nb_measures = 1
>>>nb_gap = 0
>>>
>>>try:
>>>   # Type [CTRL][C] to exit from loop
>>>   while True:
>>>      request = REQUEST_CH0[:]
>>>      digitalTemperatureCh0 = get_digit_temp(request, SPEED_STANDAR )
>>>#      print( f"TCh0 = {digitalTemperatureCh0:5.3f}")
>>>
>>>      request = REQUEST_CH1[:]
>>>      digitalTemperatureCh1 = get_digit_temp(request, SPEED_STANDAR )
>>>#      print( f"TCh1 = {digitalTemperatureCh1:5.3f}")
>>>
>>>      #
>>>      gap = digitalTemperatureCh0 - digitalTemperatureCh1
>>>#      gap = 0
>>>      if gap != 0 :
>>>          nb_gap = nb_gap + 1
>>>          if gap > 0 :
>>>              print(f"measure n°{nb_measures:7} nb_gap = {nb_gap:7}   Tch0 - Tch1 = +{gap:2.4f}  p = {nb_gap*100/nb_measures:4.3} %")
>>>          else:
>>>              print(f"measure n°{nb_measures:7} nb_gap = {nb_gap:7}   Tch0 - Tch1 = -{abs(gap):2.4f}  p = {nb_gap*100/nb_measures:4.3} %")
>>>
>>>      nb_measures = nb_measures + 1
>>>      time.sleep(0.01)                               # wait one second
>>>
>>>except:
>>>   print(f"\n\nmeasure n°{nb_measures:<7} nb_gap = {nb_gap:<7}  p = {nb_gap*100/nb_measures:4.3} <br><br>\n\n " )
>>>````
>>></details>
>>>
>>><details>
>>>   <summary>Pourquoi y a t'il un écart systématique de 0.3223 °C ?</summary><br>
>>>
>>>- La valeur du $gap$ est : <br>
>>>    - $gap = digitalTemperature_{ch0} - digitalTemperature_{ch1}$.<br><br>
>>>    - $gap = (\frac{(digitalTension_{ch0} - OFFSET)}{SENSIBILITE}) - (\frac{(digitalTension_{ch1} - OFFSET)}{SENSIBILITE})$<br><br>
>>>    - $gap = \frac{(digitalTension_{ch0} - digitalTension_{ch1}}{SENSIBILITE}$<br><br>
>>>    - $gap = \frac{(digitalTension_{ch0} - digitalTension_{ch1}}{SENSIBILITE}$<br><br>
>>>- Or<br>
>>>
>>>    -  $digitalTension = (digitalValue[0] <<8 | digitalValue[1])  * \frac{V_{REF}}{NB_{OFSAMPLES}}$
>>>    -  $digitalTension = (digitalValue[0] <<8 | digitalValue[1])  * \frac{3300}{1024}$
>>>    -  $digitalTension = (digitalValue[0] <<8 | digitalValue[1])  * 3,222656$<br><br>
>>>- Ecrivons : <br>                                   
>>>    -  $digitalTension_{CH0} \approx digit_{CH0} * 3,222656...$
>>>    -  $digitalTension_{CH1} \approx{digit_{CH1}} * 3,222656...$<br><br>
>>>- Alors
>>>    - $gap = \frac{(digit_{CH0} * 0,3222656) - digit_{CH1} * 3,222656)}{SENSIBILITE}$<br><br>
>>>    - $gap = \frac{(digit_{CH0} -  digit_{CH1}) * 3,222656}{10}$<br><br>
>>>    - $gap = (digit_{CH0} -  digit_{CH1}) * 0,3222656$<br><br>
>>>- Par conséquent :
>>>    - ***Si*** $gap = \pm{0,3223}$ soit $\approx{\pm{0,3222656...}} \implies{digit_{CH0} - digit_{CH1}} = \pm{1}$ $bit$ $soit$ $\pm{1}$ $LSB$<br><br>
>>>
>>>- C'est exactement ce qu'indiquent les caractéristiques du constructeur à la rubrique ***ACCURACY*** :<br><br>
>>>
>>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/level3_ch0ch1_connected_test_1_Accuracy.png)<br><br> 
>>></details>
>>
>></details>
>>
>><details>
>>   <summary>Observation de l'effet du paramettre $vitesse$ sur les mesures</summary><br>
>> 
>>><details>
>>>   <summary>Ce que fait le code</summary><br>
>>>
>>>- Toutes les secondes une mesure est réalisée sur le canal ***CH0***, suivie d'une mesure sur le canal ***CH1***.<br>
>>>  Cet enchaînement s'appelle un ***couple de mesures***.
>>>- Le canal ***CH0*** est la référence. La vitesse de digitalisation appliquée est toujours de $4000 Hz$.
>>>- Par contre la vitesse de digitalisation varie à chaque mesure sur le canal ***CH1***
>>>- A chaque couple de mesures une ligne s'affiche comme ci-dessous.
>>>  Il y a deux séries de mesures représentées côte à côte)
>>><br><br>
>>>
>>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/level3_ch0ch1_connected_test_2.png)<br><br> 
>>>
>>>- On remarque que :
>>>   - Les écarts de température dépendent de la vitesse
>>>   - la valeur absolue d'un écart qui est égale à ($0.3223$) ou à $0$ indique un couple de mesures correct.
>>>   - la valeur absolue d'un écart qui est supérieure à ($0.3223$) indique un couple de mesures ***incorrect***.<br>
>>>     La vitesse n'est pas adaptée.<br><br>
>>>- On en conclu que si $vitesse \in [400Hz, 40KHz]$ il est certain que la digitalisation est fiable.<br><br>
>>></details>   
>>>
>>><details>
>>>   <summary>Le code</summary><br>
>>>
>>>````python
>>>import spidev
>>>import time
>>>
>>>#Constants & parameters
>>>CE0       = 0  #  CE0  of  RPiB3+ connected on  CS of MCP3002
>>>SPI_BUS_0 = 0  #  system device either /dev/spidev0.0  or  /dev/spidev0.1
>>>
>>>SPEED_STANDAR = 4000    # Hz
>>>REQUEST_CH0 = [ 0x60, 0x00 ] # query to obtain the digitalization voltage on CH0
>>>REQUEST_CH1 = [ 0x70, 0x00 ] # query to obtain the digitalization voltage on CH0
>>>
>>>RESOLUTION    =  10             # bits
>>>NB_OF_SAMPLES = 2**RESOLUTION
>>>OFFSET        =  500.0          # mV
>>>V_REF         = 3300.0          # mV
>>>SENSIBILITE   =   10.0          # mV/°C
>>>
>>># Create instance
>>>spi = spidev.SpiDev()
>>>
>>># Open /dev/spidev0.0  with   CE0 -> CS of MCP3002
>>>spi.open( SPI_BUS_0, CE0 )
>>>
>>>#  Return the digitalized temperature directly
>>>def get_digit_temp( request, speed ):
>>>  digitalValue = spi.xfer2( request , speed)
>>>  digitalTension = (digitalValue[0] <<8 | digitalValue[1])  * V_REF / NB_OF_SAMPLES
>>>  digitalTemperature = (digitalTension - OFFSET) / SENSIBILITE
>>>  return digitalTemperature
>>>
>>># Main
>>>#------
>>>print("\n\n   Speed      TCH0  \t TCH1\t   GAP")
>>>print("   (Hz)       (°C)\t (°C)\t   (°C)")
>>>print("----------------------------------------------")
>>>
>>> # From 4Hz to 32MHz
>>>for speed in [ 2, 4, 20, 40, 200, 400, 2000, 4000, 20000, 32000, 40000, 64000, 80000, 160000, 200000, 400000, 2000000, 4000000, 20000000, 32000000 ] :
>>>    request = REQUEST_CH0[:]
>>>    digitalTemperatureCh0 = get_digit_temp(request, SPEED_STANDAR )
>>>
>>>    request = REQUEST_CH1[:]
>>>    digitalTemperatureCh1 = get_digit_temp(request, speed )
>>>
>>>    # Tag the reference speed
>>>    if speed == 4000:
>>>        print( f"{speed:8.0f} ref ", end = '')
>>>    else:
>>>        print( f"{speed:8.0f}     ", end = '')
>>>
>>>    print( f"{digitalTemperatureCh0:5.3f}\t{digitalTemperatureCh1:5.3f}\t", end ='')
>>>    gap = digitalTemperatureCh0 - digitalTemperatureCh1
>>>    if gap > 0 :
>>>        print(f"+{gap:2.4f}  ")
>>>    else:
>>>        print(f"-{abs(gap):2.4f} ")
>>>
>>>    time.sleep(1.0)                               # wait one second
>>>print()
>>>````
>>>
>>></details> 
>></details>
>>
>></details>
>   
></details>

</details>
<details>
   <summary>ETUDE DU MONTAGE AVEC $CH0$ et  $CH1$ CONNECTÉS A DES TMP36 DIFFÉRENTS</summary><br>

><details>
>  <summary><b>But de ce montage.</b></summary><br>
>
>- Ce montage va permettre d'observer le comportement des deux TMP36.<br><br>
>   - A température constantes quelles mesures obtient on pour chaque TMP36?<br><br>
>   - A température variable quelles sont les constantes de temps obtenues?
></details>

><details>
>  <summary><b>Cas à température constante.</b></summary><br>
>   - A développer.
></details>   
>
><details>
>   <summary><b>Cas à température Variable.</b></summary><br>
>
>><details>
>>   <summary><b>Principe.</b></summary><br>
>>Dans cette partie du T.P. nous voulons une estimation du temps de réponse thermique du $TMP36$.<br>
>>Le montage à deux TMP36 permet cela :<br>
>>
>>  - Nous prendrons le TMP36 du canal 0 comme référence de température (quasi constante).<br>
>>  - Nous ferons évoluer uniquement la température du TMP36 du canal 1.<br>
>>
>>La comparaison des températures mesurées au fils du temps permet d'obtenir le temps de réponse.<br><br>
>>
>>Les mesures se déroulerons de la façon suivante :<br><br>
>>- Phase de ***montée en température*** : <br>
>>     -1 On laisse les deux $TMP36$ se stabiliser.<br>
>>     -2 On lance le programme de mesure.<br>
>>     -3 On chauffe avec les doigts le $TMP36$ connecté sur le canal ***CH1***.<br>
>>     -4 Au bout de 15 secondes le programme affiche un message indiquant que l'on passe en mode refroidissement.<br>
>>
>>- Phase de ***refroidissement*** : <br>
>>     -5 On relache le $TMP36$$<br>
>>     -6 Au bout de 60 secondes le programme affiche un message de fin de mesures.<br>
>>
>>- Phase d'***enregistrement des résultats*** : <br>
>>     -7 Les mesures de la phase 1 sont copiées dans **mesure_rise.txt** du répertoire courant.<br>
>>     -8 Les mesures de la phase 2 sont copiées dans **mesure_cooling.txt** du répertoire courant.<br> 
>></details>   
>>
>><details>
>>   <summary><b>Mise en oeuvre.</b></summary><br>
>>
>>><details>
>>>   <summary><b>Précautions au moment de l'obtention des mesures.</b></summary><br>
>>>  L'expérience, et la mise au point ont montré que :<br>
>>>
>>>- Lors de la phase de montée en température il faudra absolument éviter de toucher le TMP36 de référence.<br>
>>>
>>>- Il faut faire le maximum de mesure dans le temps imparti, et réduire au maximum le temps de digitalisation.<br>
>>>     - Le temps choisi entre deux mesures est de $10$ $ms$.<br>
>>>     - La fréquence d'horloge ***SPI*** choisie est de $40$ $KHz$.<br>
>>>
>>>- Le code n'étant pas compilé, celui-ci doit être le plus efficace possible.<br>
>>>     - Code minimaliste.<br>
>>>     - Pas d'affichage des résultats à l'écran au fils des mesures.<br>
>>>     - Pas d'enregistrement des résultats de mesures dans un fichier au fils des mesures. <br>
>>>
>>>- Du fait que la mesure de température est calculée à partir de la digitalisation fournie par le MCP3002<br>
>>>  il n'est pas nécessaire de mémoriser les température ( float ) mais uniquement le code de digitalisation ( octet )
>>>     - Le code doit mémoriser les valeurs $Dij0$ et $Dij1$ obtenues respectivement sur ***CH0*** et ***CH1*** du **MCP3002**.<br>
>>>     - Le code doit mémoriser la référence temporelle associée issue de l'horloge système. <br>
>>>
>>>- Chaque mesure aura 3 champs de données.<br>
>>>     - Les deux premiers champs correspondent à $Dij0$ et $Dij1$; dans cet ordre.<br>
>>>     - Le dernier champ est la référence temporelle correspondant au début de la digitalisation sur le canal ***CH1***.<br>
>>>       Cette référence ne sera pas absolue, mais le temps écoulé depuis la première mesure.
>>>     - Les champs seront séparés par le caractère $;$<br>
>>>
>>>     -  $mesure(t_{I}) ::= Dij0(t_{I})$ $;$ $Dij1(t_{I})$ $;$ $t_{I}$ 
>>></details>
>>>
>>><details>
>>>   <summary><b>Prétraitement des données avant l'interprétation des mesures.</b></summary><br>
>>>
>>>- Sachant que la digitalisation est à $\pm1$ bit il faudra ***normaliser/corriger*** les champs $Dij0$ et $Dij1$:<br><br>
>>>    - Si pour $t_{I} \in [t_{0},t_{FINAL}]$ $Dij0(t_{I}) = M$  mais que $\exists$ quelques $t_{Q} \subset [t_{0},t_{FINAL}]$ tel que  $Dij0(t_{Q}) = M\pm1$<br>
>>>          alors il faut corriger $Dij0(t_{Q}) = M$.<br><br>
>>>     - Si pour $t_{I} \in [t_{a},t_{b}]$ $Dij1(t_{I}) = N$  mais que $\exists$ quelques $t_{P} \subset [t_{a},t_{b}]$ tel que  $Dij1(t_{P}) = N\pm1$<br>
>>>       alors il faut corriger $Dij1(t_{P}) = N$.<br>
>>></details>
>>>
>>><details>
>>>   <summary><b>Modélisation thermique constructeur du TMP36.</b></summary><br>
>>>
>>>- Dans la documention constructeur du TMP36, au paragraphe ***THERMAL ENVIRONMENT EFFECTS*** (page 9) se trouve la modélisation thermique de ce composant.<br>
>>>  Il y est précisé également la définition du temps de réponse thermique :<br>
>>>
>>>````
>>>The thermal capacity of CC varies with the measurement medium because
>>>   it includes anything in direct contact with the package.
>>>In all practical cases, the thermal capacity of CC is the limiting factor
>>>   in the thermal response time of the sensor and can be represented 
>>>   by a single-pole RC time constant response.
>>>The thermal time constant of a temperature sensor is defined as the time required
>>>   for the sensor to reach 63.2% of the final value for a step change in the temperature.
>>>````
>>>
>>>````   
>>>La capacité thermique du CC varie en fonction du milieu physique où à lieu la mesure car
>>>   cela inclut tout ce qui est en contact direct avec le boitier.
>>>Dans tous les cas pratiques, la capacité thermique du CC est le facteur qui limite
>>>   le temps de réponse thermique du capteur. Ce phénomène peut être modélisé
>>>   par une équation différentielle à un pôle de type RC où RC est la constante de temps.
>>>La constante de temps thermique d'un capteur de température est définie comme le temps 
>>>   nécessaire au capteur pour atteindre 63,2 % de la valeur finale pour un
>>>   changement en échelon de la température.
>>>````
>>>
>>>
>>></details>   
>>>
>>><details>
>>>   <summary><b>Modélisation thermique adoptée.</b></summary><br>
>>>   
>>>- Pour la partie montée en température nous utiliserons le modèle :<br><br>
>>>  $N(t)=N_{Max}*(1-\exp(-\frac{t}{\tau}))$<br>
>>>
>>>  |Paramètre|Signification|
>>>  |---|---|
>>>  | $N(t)$ | Valeur digitale au temps $t$ avec $t \in [0, t_{Final}]$ |
>>>  | $N_{Max}$ | Valeur digitale maximal atteinte|   
>>>  | $\tau$ | temps de réponse thermique de montée en température recherché|
>>>  <br>
>>>   
>>>- Pour la partie de refroidissement nous utiliserons le modèle :<br><br>
>>>  $N(t)=N_{Max}*(-\exp(-\frac{t}{\tau}))$<br>
>>>
>>>  |Paramètre|Signification|
>>>  |---|---|
>>>  | $N_{t}$ | Valeur digitale au temps $t$ avec $t \in [0, t_{Final}]$ |   
>>>  |$N_{Max}$ | Valeur digitale maximal avant refroidissement|
>>>  |$\tau$ | temp de réponse thermique de refroidissement recherché|
>>></details>   
>>>
>>><details>
>>>   <summary><b>Exemple du contenu des fichiers de résultats.</b></summary><br>
>>>
>>>````
>>>==> mesure_rise.txt <==
>>>Dij0;Dij1;time
>>>208;  213;0.00001 
>>>207;  212;0.01036
>>>206;  212;0.02057
>>>208;  213;0.03077
>>>208;  212;0.04097
>>>206;  212;0.05117
>>> ........
>>>220;  245;14.93956
>>>220;  245;14.94991
>>>220;  245;14.96025
>>>220;  245;14.97060
>>>221;  247;14.98094
>>>220;  245;14.99129
>>>220;  245;15.00163   
>>>
>>>==> mesure_cooling.txt <==
>>>Dij0;Dij1;time
>>>220;  245;0.00001    
>>>220;  245;0.01038
>>>220;  245;0.02076
>>>220;  246;0.03110
>>>220;  245;0.04145
>>>2220;  245;0.05180
>>>  .........
>>>219;  225;59.94911
>>>220;  225;59.95946
>>>220;  225;59.96981
>>>2219;  225;59.98015
>>>218;  224;59.99051
>>>218;  224;60.00083   
>>>````   
>>></details>
>> 
>></details>  
>>
>><details>
>>   <summary><b>Le code.</b></summary><br>
>>
>>````python
>>import spidev
>>import time
>>import io
>>
>>#Constants & parameters
>>CE0       = 0  #  CE0  of  RPiB3+ is connected on  CS of MCP3002
>>SPI_BUS_0 = 0  #  system device either /dev/spidev0.0  or  /dev/spidev0.1
>>
>>SPEED_STANDAR = 4000     # Hz
>>SPEED_FAST    = 40000    # Hz
>>REQUEST_CH0 = [ 0x60, 0x00 ] # query to obtain the digitalization voltage on CH0
>>REQUEST_CH1 = [ 0x70, 0x00 ] # query to obtain the digitalization voltage on CH1
>>
>>RESOLUTION    =  10             # bits
>>NB_OF_SAMPLES = 2**RESOLUTION
>>
>>TIME_PHASE_RISING  = 15.0       # second
>>TIME_PHASE_COOLING = 60.0       # second
>>WAITING_TIME       = 0.00863    # second to obtain 10 ms beetwen 2 maesures
>>TITLE              = "Dij0;Dij1;time\n"
>>
>># Create instance
>>spi = spidev.SpiDev()
>>
>># Open /dev/spidev0.0  with   CE0 -> CS of MCP3002
>>spi.open( SPI_BUS_0, CE0 )
>>
>># Return the digitalized value from chanel 0 or 1 of MCP3002 
>>def get_digitalValue( request, speed ):
>>   demande = request[:]
>>   reponse =  spi.xfer2( demande , speed)
>>   return (reponse[0] <<8 | reponse[1])
>>
>>#---------------------
>># MAIN MAIN MAIN MAIN
>>#---------------------
>>
>>print("\n START TEMPERATURE RISE PHASE (touch the TMP36)\n")
>>
>>list_mesures_up = []  # List containing all the measurements
>>ti = 0                # Time reference of the ith measure
>>to = time.time()      # Time reference for starting measurements
>>while ti < TIME_PHASE_RISING:
>>   
>>   ti = time.time() - to
>> 
>>   digitalValueCH1 = get_digitalValue(REQUEST_CH1, SPEED_FAST )
>>   digitalValueCH0 = get_digitalValue(REQUEST_CH0, SPEED_FAST )
>>
>>   list_mesures_up.append( [digitalValueCH0, digitalValueCH1, ti])
>>   
>>   time.sleep(WAITING_TIME) 
>>
>>
>>print("\n START COOLING PHASE (no longer touches the TMP36)\n")
>>
>>list_mesures_down = []  # List containing all the measurements
>>ti = 0                  # Time reference of the ith measure
>>to = time.time()        # Time reference for starting measurements
>>while ti < TIME_PHASE_COOLING:
>>
>>   ti = time.time() - to
>>
>>   digitalValueCH1 = get_digitalValue(REQUEST_CH1, SPEED_FAST )
>>   digitalValueCH0 = get_digitalValue(REQUEST_CH0, SPEED_FAST )
>>
>>   list_mesures_down.append( [digitalValueCH0, digitalValueCH1, ti])
>>
>>   time.sleep(WAITING_TIME)
>> 
>>print("\n MEASURES DONE \n")
>>print(" START MAKE OUTPUT FILES\n")
>>
>># Create output file  rise
>>out_file = io.open( "./mesure_rise.txt", "w")
>>out_file.write(TITLE)
>>
>>for mesure in list_mesures_up : 
>>   out_file.write(f"{mesure[0]:5};{mesure[1]:5};{mesure[2]:6.5f}\n")
>>out_file.close() 
>>
>># Create output file cooling 
>>out_file = io.open( "./mesure_cooling.txt", "w")
>>out_file.write(TITLE)
>>
>>for mesure in list_mesures_down :
>>   out_file.write(f"{mesure[0]:5};{mesure[1]:5};{mesure[2]:6.5f}\n")
>>out_file.close()
>>
>>print(" FINISH \n")
>>````
>></details>
>>
>>
>><details>
>>   <summary><b>Traitement des données</b></summary>
>>
>>- Les données seront traitée sous ***Excel***.<br>
>>   - [Traitement de la phase de monté en température](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_3/FR_EXCEL_WARM_UP.md) .<br>
>>   - [Traitement de la phase de refroidissement](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_3/FR_EXCEL_COOLING.md).<br><br>
>>
>>- On peut déjà donné un exemple de traitement correspondant à la phase de monté en température :<br><br>
>>
>>    - En bleu les mesures du $Gap(t_{i})=Dij1(t_{i})-Dij0(t_{i})$  exprimée en bits.<br>
>>    - En rouge la fonction $y_{Model}(t)=N*(1-(\exp(-\frac{t}{\tau}))$ de la modélisation. Avec $N=22$ bits, et $\tau=3,1255$)
>>    - Le carré vert correspond à $y(\tau)=63,2$ % de $N$ soit $14$ bits.<br><br>
>> 
>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/level3_ch0ch1_disconnected_test_2.png)
>>
>></details> 
>>
>><details>
>>   <summary><b>Conclusion</b></summary>
>>
>>- ***Reproductibilité des mesures***<br>
>>
>>   - La phase de monté en température est très dépendante de la façon dont on tient le capteur de température.<br>
>>     Après de nombreux essais, la valeur de $\tau = 3,7$ s trouvée initialement varie entre $2,8$ et $4,2$ secondes.<br>
>>     Soit une ereur de $\pm20$%<br><br>
>>
>>   - La phase de refroidissement ne dépend plus de la façon de tenir le TMP36.<br>
>>     Après de nombreux essais, la valeur de $\tau = 23$ s trouvée initialement varie entre $18,5$ et $23$ secondes.<br>
>>     Soit une erreur de $\pm10$%<br><br>
>>
>>- ***Pourquoi les valeurs de*** $\tau$ ***sont différentes entre les deux phases.***<br>
>>   - Le constructeur indique que thermiquement son composant peut être modélisé par une fonction de transfert à $deux$ $pôles$.<br>
>>     Mais en fait cela peut se ramener à une fonction de transfert à seul pôle qui correspond au comportement thermique du boitier.<br>
>>   - Donc selon la qualité du contact thermique entre la source chaude/froide et le boitier, les conditions ne sont pas les mêmes.
>>     D'aileurs les courbes de mesure du constructeur indique les conditions de mesure : 
>>       - Bain d'huile.
>>       - Radiateur monté sur le TMP36.
>>       - Refroidissement avec souflage d'air.<br>
>>
>>- ***Les précaution d'utilisation du TMP36.***
>>   - Ce composant ne pourra pas mesurer des variation de température trop rapide.<br>
>>     Il sera idéal pour mesurer la température d'un logement par exemple.<br>
>>
>>- ***Critique de cette méthode de mesure.***
>>    - Nous avons réalisé un calcul sur des tensions numérisées.<br>
>>      Ce qui a doublé l'erreurs dues au pas denumérisation ( $\pm2$ bits)<br>
>>
>>- ***Solution.***
>>    - le MCP3002 à la posibilité de mesurer des grandeurs de façon différentielle.<br>
>>      La numérisation se fait directement sur le signal analogique ($Canal_{1} - Canal_{0}$).<br>
>>      Il n'y a plus de calcul à faire, et l'erreur due à la numérisation reste à  $\pm1$ bits.<br>
>>      C'est l'objet du T.P. de niveau ⭐⭐⭐⭐
>></details> 
></details>

</details>
