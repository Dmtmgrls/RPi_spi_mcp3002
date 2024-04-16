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
>>       -1    On laisse les deux $TMP36$ se stabiliser en température.<br>
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
>>>     - La fréquence d'horloge ***SPI*** choisie est de $4$ $KHz$.<br>
>>>
>>>- Le code n'étant pas compilé, celui-ci doit être le plus efficace possible.<br>
>>>     - Code minimaliste.<br>
>>>     - Pas d'affichage des résultats à l'écran au fils des mesures.<br>
>>>     - Pas d'enregistrement des résultats de mesures dans un fichier au fils des mesures. <br>
>>></details>
>>>
>>><details>
>>>   <summary><b>Quelles sont les mesures?</b></summary><br>
>>>
>>>- Du fait que la mesure de température est calculée à partir de la digitalisation fournie par le MCP3002,<br>
>>>  il n'est pas nécessaire de mémoriser les températures ( float ) mais uniquement le code de digitalisation ( octet )<br><br>
>>>  <b><ins>Remarque : </ins></b>Dans ce qui suit, le préfix $Dig$ signifie $Digitalisation$)<br>
>>>
>>>     - Le code doit mémoriser la référence temporelle associée issue de l'horloge système. <br>
>>>     - Le code doit mémoriser les valeurs $Dig_{0}$ et $Dig_{1}$ obtenues respectivement sur ***CH0*** et ***CH1*** du **MCP3002**.<br>
>>>     - Le code doit mémoriser les valeurs $Dig_{Diff0}$ obtenue respectivement sur ***CH+*** et ***CH-*** du **MCP3002**.<br>
>>>     - Le code doit mémoriser les valeurs $Dig_{Diff1}$ obtenue respectivement sur ***CH-*** et ***CH+*** du **MCP3002**.<br>
>>>
>>>- Chaque mesure correspondra à 4 numérisations faites dans l'ordre suivant :<br>
>>>    - digitalisation asymétrique sur $CH1$<br> 
>>>    - digitalisation différentielle sur $CH0+$.<br>
>>>    - digitalisation différentielle sur $CH1+$.<br>
>>>    - digitalisation asymétrique sur $CH0$<br>
>>></details>
>>>
>>><details>
>>>   <summary><b>Quelles sont les champs des fichiers de sortie?</b></summary><br>
>>>
>>>- Tous les champs seront séparés par une virgule : $,$<br>
>>>
>>>-  Sachant que les données seront traitées dans un tableur (Excel), nous aurons besoins de colonnes affectées aux calculs.<br>
>>>   Nous allons préformater le fichier de telle sorte que pour chaque mesure nous ayons des champs de type ***Data*** et des champs de type ***Calcul*** :<br><br> 
>>>
>>>    | Rang | Type | Libellé | Description |
>>>    |---|---|---|---|
>>>    | 1 | Data | ***time*** | Référence temporelle de la mesure.<br>Entre chaque mesure s'écoulera un délais de 50 ms environ |
>>>    | 2 | Calcul | ***time_D*** | Les premières mesures dépendent de la réactivité de l'opérateur.<br>Après élimination des ces premières mesures il faut recalculer les références temporelles.<br>$$time\\_D = time - Decalage$$
>>>    | 3 | Data | ***Dig_0*** | Digitalisation $Dig_{0}$ du canal $0$ en mode asymétrique. |
>>>    | 4 | Data | ***Dig_1*** | Digitalisation $Dig_{1}$ du canal $1$ en mode asymétrique. |
>>>    | 5 | Calcul | ***Delta*** | $$Delta = Dig\\_1 - Dig\\_0$$ |
>>>    | 6 | Calcul | ***Model_Asym*** | Calcul de modélisation en mode asymétrique.<br>$$Model\\_Asym = F(\frac{time\\_D}{\tau})$$|
>>>    | 7 | Calcul | ***Ecart*** | Calcul des moindres carrés en mode asymétrique.<br>$$Ecart = (Delta - Model\\_Asym)^2$$ | 
>>>    | 8 | Data | ***Dig_Diff0*** | Digitalisation $Dig_{Diff0}$ du canal $CH0+$ en mode différentiel.<br>Normalement toujours égal à $0$ |
>>>    | 9 | Data | ***Dig_Diff1*** | Digitalisation $Dig_{Diff1}$ du canal $CH1+$ en mode différentiel. | 
>>>    | 10 | Calcul | ***Model_Diff*** | Calcul de modélisation en mode différentiel..<br>$$Model\\_Diff = G(\frac{time\\_D}{\tau})$$ |
>>>    | 11 | Calcul | ***Ecart*** | Calcul des moindres carrés en mode différentiel.<br>$$Ecart = (Dig\\_Diff1 - Model\\_Diff)^2$$ | 
>>></details>
>>>
>>><details>
>>>   <summary><b>Prétraitement des données avant l'interprétation des mesures.</b></summary><br>
>>>
>>>- Sachant que la digitalisation est à $\pm1$ bit il faudra ***normaliser/corriger*** les champs $Dig_{0}$ et $Dig_{1}$:<br><br>
>>>    - Si pour $t_{I} \in [t_{0},t_{FINAL}]$ $Dig_{0}(t_{I}) = M$  mais que $\exists$ quelques $t_{Q} \subset [t_{0},t_{FINAL}]$ tel que  $Dig_{0}(t_{Q}) = M\pm1$<br>
>>>          alors il faut corriger $Dig_{0}(t_{Q}) = M$.<br><br>
>>>     - Si pour $t_{I} \in [t_{a},t_{b}]$ $Dig_{1}(t_{I}) = N$  mais que $\exists$ quelques $t_{P} \subset [t_{a},t_{b}]$ tel que  $Dig_{1}(t_{P}) = N\pm1$<br>
>>>       alors il faut corriger $Dig_{1}(t_{P}) = N$.<br>
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
>>>time,time_D,Dig_0,Dig_1,Delta,Model_Delta,Ecart,Dig_Diff0,Dig_Diff1,Model_Diff,Ecart
>>>0.00000,,  231,  232,,,,    0,    0,,
>>>0.04625,,  231,  232,,,,    0,    0,,
>>>0.09239,,  232,  232,,,,    0,    0,,
>>>0.13852,,  232,  232,,,,    0,    0,,
>>>0.18464,,  232,  232,,,,    0,    0,,
>>> ........
>>>14.81087,,  224,  251,,,,    0,   26,,
>>>14.86423,,  224,  252,,,,    0,   26,,
>>>14.91757,,  224,  252,,,,    0,   26,,
>>>14.97091,,  224,  251,,,,    0,   26,,
>>>15.02426,,  224,  252,,,,    0,   26,,
>>>
>>>==> mesure_cooling.txt <==
>>>time,Dig_0,Dig_1,Delta,Dig_Diff0,Dig_Diff1
>>>0.00001,,  224,  249,,,,    0,   23,,
>>>0.05346,,  224,  248,,,,    0,   24,,
>>>0.10702,,  224,  249,,,,    0,   23,,
>>>0.16045,,  224,  249,,,,    0,   23,,
>>>0.21248,,  224,  248,,,,    0,   23,,
>>>  .........
>>>69.80008,,  224,  227,,,,    0,    1,,
>>>69.85342,,  224,  227,,,,    0,    2,,
>>>69.90678,,  224,  227,,,,    0,    2,,
>>>69.96041,,  224,  227,,,,    0,    1,,
>>>70.01459,,  224,  227,,,,    0,    2,,
>>>```` 
>>>
>>>- Ce qui, dans le tableur, donnera le format suivant : <br>
>>>
>>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Exemple_file_Excel_step4_Soft.png)
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
>>SPEED_FAST    = 4000     # Hz
>>REQUEST_CH0 = [ 0x60, 0x00 ] # query to obtain the digitalization voltage on CH0
>>REQUEST_CH1 = [ 0x70, 0x00 ] # query to obtain the digitalization voltage on CH1
>>REQUEST_DIFF_CH0 = [ 0x48, 0x00 ] # query to obtain the digitalization voltage on CH0 - CH1
>>REQUEST_DIFF_CH1 = [ 0x58, 0x00 ] # query to obtain the digitalization voltage diff  CH1 - CH0
>>
>>RESOLUTION    =  10             # bits
>>NB_OF_SAMPLES = 2**RESOLUTION
>>
>>TIME_PHASE_RISING  = 15.0       # second
>>TIME_PHASE_COOLING = 70.0       # second
>>WAITING_TIME       = 0.03416    # second to obtain 50  ms beetwen 2 maesures
>>TITLE              = "time,time_D,Dig_0,Dig_1,Delta,Model_Delta,Ecart,Dig_Diff0,Dig_Diff1,Model_Diff,Ecart\n"
>>NAME_FILE_RISE     = "./mesures_rise.txt" 
>>NAME_FILE_COOLING  = "./mesures_cooling.txt"
>>
>># Create instance
>>spi = spidev.SpiDev()
>>
>># Open /dev/spidev0.0  with   CE0 -> CS of MCP3002
>>spi.open( SPI_BUS_0, CE0 )
>>
>># Return the digitalized differential value from chanels of MCP3002 
>>def get_digitalValue( request, speed ):
>>  demande = request[:]
>>  reponse =  spi.xfer2( demande , speed)
>>  return (reponse[0] <<8 | reponse[1])
>>
>>
>>#---------------------
>># MAIN MAIN MAIN MAIN
>>#---------------------
>>
>>print(f"\n START TEMPERATURE RISE PHASE (touch the TMP36 {TIME_PHASE_RISING} seconds)\n")
>>
>>list_mesures_up = []    # List containing all the measurements
>>ti = 0                  # Time reference of the ith measure
>>to = time.time()        # Time reference for starting measurements
>>while True:
>>  
>>  ti = time.time() - to
>>
>>  digitalValue_CH1 = get_digitalValue( REQUEST_CH1,  SPEED_FAST )
>>  digitalDiffValue_CH0 = get_digitalValue( REQUEST_DIFF_CH0,  SPEED_FAST )
>>  digitalDiffValue_CH1 = get_digitalValue( REQUEST_DIFF_CH1,  SPEED_FAST )
>>  digitalValue_CH0 = get_digitalValue( REQUEST_CH0,  SPEED_FAST )
>>
>>  list_mesures_up.append( [ti, digitalValue_CH0, digitalValue_CH1, digitalDiffValue_CH0, digitalDiffValue_CH1])
>>  
>>  time.sleep(WAITING_TIME) 
>>  
>>  if ti > TIME_PHASE_RISING:
>>    break
>>
>>
>>print(f"\n START COOLING PHASE (no longer touches the TMP36 {TIME_PHASE_COOLING} seconds)\n")
>>time.sleep(3) # perator response
>>
>>list_mesures_down = []  # List containing all the measurements
>>tj = 0                  # Time reference of the jth measure
>>to = time.time()        # Time reference for starting measurements
>>while True:
>>
>>  tj = time.time() - to
>>
>>  digitalValue_CH1 = get_digitalValue( REQUEST_CH1,  SPEED_FAST )
>>  digitalDiffValue_CH0 = get_digitalValue( REQUEST_DIFF_CH0,  SPEED_FAST )
>>  digitalDiffValue_CH1 = get_digitalValue( REQUEST_DIFF_CH1,  SPEED_FAST )
>>  digitalValue_CH0 = get_digitalValue( REQUEST_CH0,  SPEED_FAST )
>>
>>  list_mesures_down.append( [tj, digitalValue_CH0, digitalValue_CH1, digitalDiffValue_CH0, digitalDiffValue_CH1])
>>
>>  time.sleep(WAITING_TIME)
>>
>>  if  tj > TIME_PHASE_COOLING:
>>     break 
>>
>>
>>print("\n MEASURES DONE \n")
>>print(" START MAKE OUTPUT FILES\n")
>>
>># Create output file  rise
>>out_file = io.open( NAME_FILE_RISE, "w")
>>out_file.write(TITLE)
>>
>>for mesure in list_mesures_up : 
>>  out_file.write(f"{mesure[0]:6.5f},,{mesure[1]:5},{mesure[2]:5},,,,{mesure[3]:5},{mesure[4]:5},,\n")
>>out_file.close() 
>>
>># Create output file cooling 
>>out_file = io.open( NAME_FILE_COOLING, "w")
>>out_file.write(TITLE)
>>
>>for mesure in list_mesures_down :
>>  out_file.write(f"{mesure[0]:6.5f},,{mesure[1]:5},{mesure[2]:5},,,,{mesure[3]:5},{mesure[4]:5},,\n")
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
>>   - [Traitement de la phase de monté en température](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_4/FR_EXCEL_WARM_UP.md) .<br>
>>   - [Traitement de la phase de refroidissement](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_4/FR_EXCEL_COOLING.md).<br><br>
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

