### LES NOUVEAUX PROGRAMMES
Niveau ⭐⭐⭐⭐
<br><br>

<details>
   <summary>ETUDE DU MONTAGE AVEC $CH0$ et  $CH1$ CONNECTÉS A DES TMP36 DIFFÉRENTS</summary><br>

><details>
>  <summary><b>But de ce montage.</b></summary><br>
>
>- Ce montage permet d'observer le comportement du **MCP3002** en mode $différentiel$ et $asymétrique$.<br><br>
>   - A température constantes quelles mesures obtient on pour chaque TMP36?<br><br>
>   - A température variable quelles sont les constantes de temps obtenues?
></details>
>
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
>>
>>-   Nous allons reprendre exactement ce qui a été fait dans le T.P de niveau ⭐⭐⭐ dans le cas de la température variable.<br>
>>    La différence tient au fait que chaque mesure sera réalisée "***simultanément***" en mode $Asymétrique$ et $Différentiel$.<br>
>>    Ainsi dans le T.P de niveau ⭐⭐⭐⭐ :<br>
>>
>>     -   Nous prendrons le TMP36 du canal $0$ comme référence de température (quasi constante).<br>
>>     -   Nous ferons évoluer uniquement la température du TMP36 du canal $1$.<br><br>
>>
>>-   A partir des mesures :<br>
>>
>>     -   Nous modéliserons le comportement du TMP36.<br>
>>     -   Nous déterminerons le temps de réponse thermique du TMP36.<br>
>>     -   Nous vérifierons s'il y a divergence entre le mode $asymétrique$ et le mode $différentiel$.<br><br>
>>
>>-  Les mesures se déroulerons de la façon suivante :<br>
>>
>>    -   Phase $1$ : ***montée en température*** : <br>
>>       -1    On laisse les deux $TMP36$ se stabiliser.<br>
>>       -2    On lance le programme de mesure.<br>
>>       -3    On chauffe avec les doigts le $TMP36$ connecté sur le canal ***CH1***.<br>
>>       -4    Au bout de 15 secondes le programme affiche un message indiquant que l'on passe en mode refroidissement.<br>
>>
>>    -   Phase $2$ : ***refroidissement*** : <br>
>>       -5 On relache le $TMP36$$<br>
>>       -6 Au bout de 70 secondes le programme affiche un message de fin de mesures.<br>
>>
>>    -   Phase $3$ : ***enregistrement des résultats*** : <br>
>>       -7 Les mesures de la phase 1 sont copiées dans **mesure_rise.txt** du répertoire courant.<br>
>>       -8 Les mesures de la phase 2 sont copiées dans **mesure_cooling.txt** du répertoire courant.<br> 
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
>>>     - Le temps choisi entre deux mesures est de $50$ $ms$.<br>
>>>     - La fréquence d'horloge ***SPI*** choisie est de $10$ $KHz$.<br>
>>>
>>>- Le code n'étant pas compilé, celui-ci doit être le plus efficace possible.<br>
>>>     - Code minimaliste.<br>
>>>     - Pas d'affichage des résultats à l'écran au fils des mesures.<br>
>>>     - Pas d'enregistrement des résultats de mesures dans un fichier au fils des mesures. <br>
>>>
>>>- Du fait que la mesure de température est calculée à partir de la digitalisation fournie par le MCP3002<br>
>>>  il n'est pas nécessaire de mémoriser les température ( float ) mais uniquement le code de digitalisation ( octet )<br>
>>>  (ici le préfix $Dig$ signifie $Digitalisation$
>>>     - Le code doit mémoriser les valeurs $Dig_{0}$ et $Dig_{Diff1}$ obtenues respectivement sur ***CH0*** et ***CH1*** du **MCP3002**.<br>
>>>     - Le code doit mémoriser les valeurs $Dig_{Diff0}$ obtenue respectivement sur ***CH+*** et ***CH-*** du **MCP3002**.<br>
>>>     - Le code doit mémoriser les valeurs $Dig_{Diff1}$ obtenue respectivement sur ***CH-*** et ***CH+*** du **MCP3002**.<br>
>>>     - Le code doit mémoriser la référence temporelle associée issue de l'horloge système. <br>
>>>
>>>- Chaque mesure aura 5 champs de données.<br>
>>>     - Le premier champ est la référence temporelle correspondant au début de la digitalisation sur le canal ***CH1***.<br>
>>>       Cette référence ne sera pas absolue, mais correspondra au temps écoulé depuis la première mesure.   
>>>     - Les deux champs suivants correspondent à $Dig_{0}$ et $Dig_{1}$; dans cet ordre, au mode $Asymétrique$.<br>
>>>     - Les deux champs suivants correspondent à $Dig_{Diff0}$ et $Dig_{Diff1}$; dans cet ordre, au mode $Différentiel$.<br>
>>>     - Les champs seront séparés par le caractère virgule $,$<br>
>>>
>>>     -  $mesure(t_{I})$ ::= $t_{I}$ $,$ $Dig_{0}(t_{I})$ $,$ $Dig_{1}(t_{I})$ $,$  $Dig_{Diff0}(t_{I})$ $,$ $Dig_{Diff1}(t_{I})$
>>></details>
>>>
>>><details>
>>>   <summary><b>Prétraitement des données avant l'interprétation des mesures.</b></summary><br>
>>>
>>>- Sachant que la digitalisation est à $\pm1$ bit il faudra ***normaliser/corriger*** les champs $Dig0$ et $Dig1$:<br><br>
>>>    - Si pour $t_{I} \in [t_{0},t_{FINAL}]$ $Dig0(t_{I}) = M$  mais que $\exists$ quelques $t_{Q} \subset [t_{0},t_{FINAL}]$ tel que  $Dig0(t_{Q}) = M\pm1$<br>
>>>          alors il faut corriger $Dig0(t_{Q}) = M$.<br><br>
>>>     - Si pour $t_{I} \in [t_{a},t_{b}]$ $Dig1(t_{I}) = N$  mais que $\exists$ quelques $t_{P} \subset [t_{a},t_{b}]$ tel que  $Dig1(t_{P}) = N\pm1$<br>
>>>       alors il faut corriger $Dig1(t_{P}) = N$.<br>
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
>>>Dig0;Dig1;time
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
>>>Dig0;Dig1;time
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
>>TITLE              = "Dig0;Dig1;time\n"
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
>>    - En bleu les mesures du $Gap(t_{i})=Dig1(t_{i})-Dig0(t_{i})$  exprimée en bits.<br>
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
