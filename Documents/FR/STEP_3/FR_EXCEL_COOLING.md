### EXPLOITATION DES MESURES DANS LA PHASE MONTÉ EN TEMPÉRATURE

Niveau ⭐⭐⭐

<details>
     <summary><b>Etapes de traitement des données</b></summary><br>

><details>
>     <summary><b>1 Chargement du fichier dans Excel</b></summary><br>
>
> Ouvrir $Excel$ et charger le fichier $mesure\\_ rise.txt$.<br>
> Il faut pré-formater ce fichier sur les 3 aspects suivants :<br>
>
>   - Les champs du fichier sont délimités par le caractère point-virgule $;$.
>   - Le fichier contient une en-tête $Dij0; Dij1; time$.
>   - Pour la colonnes $time$, sélectionner que le ***point*** est le **séparateur décimal**.
>
></details>
>
><details>
>     <summary><b>2 Normaliser la colonne Dij0</b></summary><br>
>
>><details>
>><summary><b>Pré traitement</b></summary><br>
>>
>>-  ( 1a ) Insérer sept lignes, en haut de feuille, pour laisser de la place pour les calculs qui suivent.
>>-  ( 1b ) Insérer une colonne $[A]$ pour laisser de la place pour les calculs qui suivent.
>>-  ( 2 ) Nommer la colonne $[E]$ par le label $Gap$. Les cellules contiendrons $Gap(t_{I}) = Dij1(t_{I}) - Dij0(t_{I})$
>>-  ( 3 ) Calculer les valeurs : Cellule $[B4] = MAX(Dij0)$ et Cellule $[B5] = MIN(Dij0)$ avec $t_{I} \in [0,0001; t_{15s}]$.
>>-  ( 4 ) En colonne $[G]$ et suivantes, calculer les distributions des valeurs de $Dij0$ comprisent entre $[MIN(Dij0),MAX(Dij0]$<br><br>
>>- Les formules sont résumées ci-dessous : <br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_a.png)<br><br>
>>
>>- Le résultat de ces formules est le suivant :<br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_b.png)<br><br>
>>  
>>   -    Les valeurs de $Dij0$ sont comprises entre 221 et 226 bits.<br>
>>   -    Les valeurs 224 et 223 sont les plus fréquentes, et rentre dans la catégorie $\pm 1 bit$.<br>
>>   -    Les autres valeurs sont **_incertaines_** puisqu'en théorie $Dij0$ est **_constante_**.<br> 
>>- Il faut maintenant répondre à la question : Quelle est la valeur $DIJ0$<br>
>>   -  telque $Dij0(t_{I}) = DIJ0$ $\forall$ $t_{I} \in [0,0001; t_{15s}]$ ?<br><br>
>></details>
>><details>
>>    <summary><b>Consolidation et courbe</b></summary><br>
>>
>>- ( 1 ) Nommer $Gap\\_glis$ la colonne $[F]$ qui calculera la moyenne glissante de $Gap$
>>- ( 2 ) Insérer la courbe suivante : <br>
>>    - Abcisses : $time$
>>    - Ordonnées : $Gap$ (en bleu), $Gap\\_glis$ en rouge.<br><br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_c.png)<br><br>
>>
>>- On constates :
>>   - Les points de mesures correspondants au $Gap$ sont très dispérsées. Cela tient aux faits que :<br>
>>
>>       - (1) $Gap = (Dij1 \pm1 bit) - ( Dij0 \pm1 bit ) \implies$ $Gap \pm2bits$.<br>
>>
>>       - (2) $Dij0$ est théoriquement **_constant_**, or certaines de ses valeurs varient de façon aberrantes.<br>
>>         Elles passent, par exemple de 222 à 226, puis reviennent à 224 en l'espace de 20 ms.<br><br>
>>
>>   - La courbe correspondant au $Gap\\_glis$ montre l'effet de la moyenne glissante qui lisse ces dispersions
>>     sans pour autant masquer la forme en escalier de la courbe associée à $Gap$.
>>
>>- Après avoir testé les valeurs de  $Dij0 \in [221, 226]$ les résultats sont les suivants :
>>   - $\forall$ $Dij0 \in [221, 222]$ alors $Gap > 0$
>>   - $\exists$ $Dij0 \in [224, 225, 226]$ tel que $Gap < 0$ 
>>   - $SI Dij0 = 223$ Alors $Gap \ge 0$ C'est la valeur la plus adaptée.
>></details>
>><details>
>>    <summary><b>Les choix.</b></summary>
>>
>>- Suite aux tests précédents, les valeurs retenues sont  :<br>
>>
>>    -   $Dij0(t_{I}) = 223$  $\forall$  $t_{I} \in [0,0001, t_{15s}]$.<br><br>
>>    -   Pour éliminer les mesures au tous début et ne garder qu'une seule valeur $Gap = 0$ on pratique un
>>        décalage temporel $t_{offset} = 0,28064$ tel que $time_{d} = t_{I}$ -  $t_{offset}$<br><br> 
>>    -   Supression des mesures $Dij0(t_{I})$  $\forall$ $t_{I} \in [0,00001; 0,28064[$<br><br>
>>    -   La moyenne glissante à pour formule :
>> $$Gap\\\_glis(t_{I}) = \sum_{i=I}^{I+8} Gap(t_{i})$$<br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_d.png)<br><br>
>>
>></details>
>><details>
>>    <summary><b>Finalisation.</b></summary>
>>
>>- Insérer une colonne entre les colonnes **[D]** et  **[E]**, puis affecter le nom $time_{d}$ à la colonne **[E]**  <br>
>>  Calculer le décalage temporel : $time_{d} = time$ -  $t\\_offset$ <br>
>>
>>- Insérer la nouvelle courbe correspondant : <br>
>>    - Abcisses : $time_{d}$
>>    - Ordonnées : $Gap$, $Gap\\_glis$.<br><br> 
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_e.png)<br><br>
>>
>></details>
></details>
>
><details>
>    <summary><b>3 Mise en oeuvre du model</b></summary><br>
>
>><details>
>>    <summary><b>Ajout de la formule mathématique du model.</b></summary><br>
>>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_g.png)<br><br>
>>
>>- Ecrire respectivement dans les cellules ***H3, H4, H5*** les labels  $\tau$, $\epsilon$ , $Gap\\\_glis\\\_Max$ <br>
>>- Nommer respectivement les cellules ***I3, I4, I5*** par __"Tau", "Epsilon" , "Gap_glis_Max"__ <br>
>>
>>- Donner à la cellule ***H8*** le titre _"model"_.
>>- Les cellules $H_{k}$ continnent la formule  $=Gap\\\_glis\\\_Max * (1 - EXP(- E_{k} / (Tau)))$<br>
>>
>>
>></details>
>>
>><details>
>>    <summary><b>Ajout des calculs des moindres carrés</b></summary><br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_2_g.png)<br><br>
>>
>> Le principe est de minimiser la ***sommes des carrés des écart*** entre le _model_ et _la moyenne glissante_.<br>
>> C'est à dire minimiser l'erreur en faisant varier la valeur de $\tau$ :
>>
>> $$erreur_{minimale} = \sum_{d=0}^{N} (model(time_{d}, \tau_{optimale})) - Gap\\\_glis(time_{d}))^{2}$$
>>
>> Pour simplifier la recherche manuelle de la valeur optimale de $\tau$ nous calculons l'effet de la variation de $\tau$ d'une valeur de $\pm\epsilon$:
>>- $K_{d} = model(time_{d}, \tau -\epsilon)) - Gap\\\_glis(time_{d})$
>>- $L_{d} = model(time_{d}, \tau )) - Gap\\\_glis(time_{d})$
>>- $M_{d} = model(time_{d}, \tau +\epsilon)) - Gap\\\_glis(time_{d})$
>><br>
>> Les formules à placer dans les celleules seront donc :
>>
>>- Pour $i \in [9, N]$<br>
>>
>>    -   Cellule **[Ki]** contient la formules Excel : $=(Gi-Gap\\_glis\\_Max*(1-exp(-Ei/(Tau-Epsilon))))^2$
>>    -   Cellule **[Li]** contient la formules Excel : $=(Gi-Hi)^2$
>>    -   Cellule **[Mi]** contient la formules Excel : $=(Gi-Gap\\_glis\\_Max*(1-exp(-Ei/(Tau+Epsilon))))^2$
>>
>>- Pour les cellules **[K6], [L6], [M6]**, on calcul la somme des carrés.<br>
>>
>>    -   Cellule **[K6]** contient la formules Excel : $=SOMME(K9:K_{N})$
>>    -   Cellule **[L6]** contient la formules Excel : $=SOMME(L9:L_{N})$
>>    -   Cellule **[M6]** contient la formules Excel : $=SOMME(M9:M_{N})$
>>
>></details>
></details>
>
</details>
<details>
    <summary><b>Estimation du temps de réponse thermique du TMP36</b></summary><br>

><details>
>    <summary><b>Méthode d'estimation</b></summary><br>
>
>- Estimation initiale :
>    - Le temps de mesure est de 15 secondes, prenons arbitrairement $\tau = 10 \pm5 secondes$<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_4_h1.png)<br>
>
>- Estimation suivante : 
>    - Le résultat précédent nous indique que $\tau_{optimal}$ est entre **0 et 5 s**. Prenons arbitrairement $\tau = 2 \pm1 s$<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_4_h2.png)<br>
>
>- Estimation suivante : 
>    -  Le résultat précédent nous indique que $\tau_{optimal}$ est entre **3 et 5 s**. Prenons arbitrairement $\tau = 4 \pm0.5 s$<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_4_h3.png)<br>
>
>- Estimation suivante :
>    -  Le résultat précédent nous indique que $\tau_{optimal}$ est entre **3.5 et 4 s**. Prenons arbitrairements $\tau = 3.8 \pm0.1 s$<br>
>       Et ainsi de suite jusqu'à ce que l'on estime que la précision est suffisante.<br><br>
>       
>- Ainsi, on trouve que $\tau_{optimal}$ s'approche de la valeur $3,6809$ soit plus raisonablement $3.7 \pm 0.01$<br><br>
>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_4_h4.png)<br>
>      
></details>
>
><details>
>    <summary><b>Résultat graphique</b></summary><br>
>
>-  En bleu la courbe de $Gap(t_{d})$ correspondant aux mesures de la digitalisation.<br><br>
>-  En rouge la courbe de la moyenne glissante des mesures qui permet de lisser les abérrations de mesures.<br><br>
>-  En vert la courbe de modélisation d'équation $Gap_{model}(t_{d}) = Gap\\\_glis\\\_Max*(1-\exp(-\frac{t_{d}}{\tau}))$ avec $Gap\\\_glis\\\_Max = 26 bits$<br><br>
>-  le carré noir représente le point modélisé correspondant au temps de réponse du TMP36 en monté en température.<br><br>
>   Ce point est obtenu pour $t_{d} = \tau$ c'est à dire $Gap_{model}(\tau) = Gap\\\_glis\\\_Max*(1-\exp(-1)) = 26*0.632 = 16.4 bits$<br><br>
> 
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_warm_up_step_4_h5.png)<br>
>
</details>>
