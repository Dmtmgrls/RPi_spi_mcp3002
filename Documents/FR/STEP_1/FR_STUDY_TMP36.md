### ETUDE  DU TMP36 NIVEAU :star:
<details>
    <summary>Etudions plus précisément le fonctionnement du <b>TMP36</b></summary><br>

> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/TMP36DZ_Fonction_Vout_Tmp.png)
>
>- Dans le shéma ci-dessus nous avons :
>     -  A gauche le convertiveur [Température / Tension ] fournissant la tension $V_{out}$.
>     -  A droite une fonction qui converti la tension $V_{out}$ en degrés centigrades, correspondant à la température mesuréee.<br><br>
>
> [Voir étude](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR/STEP_1/FR_STUDY_TMP36.md)
> 
><details>
>    <summary>Modélisation du <b>TMP36</b> par le constructeur</summary>
>
>- Le constructeur modélise sont convertiseur par :
>     -  Une fonction linéaire : $f_{model}(T) = a * T + b$
>     -  Ayant pour dimension : $[f_{model}(T)] = [mV]$ <br><br>
>
>- Dans la documentation constructeur du **TMP36** les informations essentielles sont celles-ci :
>
> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/TMP36DZ_caracteristiques.png)
>   <br>
>
>- Il est alors facile de trouver les valeurs de $a$ et de $b$ : <br><br>
>     -  Nous connaissons la pente de cette droite $a = 10$ $[\frac{mV}{°C}]$<br><br>
>     -  Nous connaissons un point de cette  droite $(25°C, 750mV)$ soit $750 = a * 25 + b \implies b = 500$ $[mV]$<br><br>
>     -  La fonction de modélisation s'écrit alors  $f_{model}(T) = Sensibility * T + Offset$ avec :
>        - $Sensibility = 10$ $[\frac{mV}{°C}]$
>        - $Offset = 500$ $[mV]$<br><br>
>
>- On peut vérifier que :
>    -  Sur plage de $-40$ à $+125$ $[°C]$ nous voyons que $V_{model}$ est sur la plage $100$ à $2000$ $[mV]$ du constructeur.<br>
>       Les calculs du tableau ci-dessous sont corrects : <br><br>
>       |  T°C |$f_{model}(T)$ |
>       |---|---|
>       |  -40 °C |100 mv |
>       |   0 °C |500 mv
>       |   **+25 °C** |:heavy_check_mark: **750 mv** |
>       |  +50 °C |1000 mv |
>       |  +125 °C |2000 mv |
><br>
></details>
>
>
><details>
>    <summary>Déterminons la fonction $f(V_{out})$ </summary><br>
>
>-  Puisque $f(V_{model}) = a*T +b$ son inverse est donc $T = \frac{f(V_{model})- b}{a}$<br><br>
>- Ainsi :<br>
>
> |  $f(V_{out}) = T = \frac{V_{out} - Offset}{Sensibility}$ en $[°C]$|
> |---|
>
></details>


</details>

