### EXPLOITATION DES MESURES DANS LA PHASE REFROIDISSEMENT

Niveau ⭐⭐⭐

<details>
     <summary><b>Etapes de traitement des données</b></summary><br>

><details>
>     <summary><b>1 Chargement du fichier dans Excel</b></summary><br>
>
> Ouvrir $Excel$ et charger le fichier $mesure\\_cooling.txt$.<br>
> Il faut pré-formater ce fichier sur les 3 aspects suivants :<br>
>
>   - Les champs du fichier sont délimités par le caractère point-virgule $;$.
>   - Le fichier contient une en-tête $Dij0; Dij1; time$.
>   - Pour la colonnes $time$, sélectionner le ***point*** en tant que **séparateur décimal**.
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
>>-  ( 3 ) Calculer les valeurs : Cellule $[B4] = MAX(Dij0)$ et Cellule $[B5] = MIN(Dij0)$ avec $t_{I} \in [0,0001; t_{60s}]$.
>>-  ( 4 ) En colonne $[G]$ et suivantes, calculer les distributions des valeurs de $Dij0$ comprisent entre $[MIN(Dij0),MAX(Dij0]$
>>-  ( 5 ) Nommer la colonne $[F]$ par le label $Gap\\\_glis$. Calculer la moyenne glissante tel que $\forall I \in [9, I_{Max}]$   $$F_{I} = \frac{1}{8}*\sum_{i=I}^{I+8} E_{I}$$<br>
>>- **Les formules sont résumées ci-dessous :** <br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_a.png)<br><br>
>>
>>- **Le résultat de ces formules est le suivant :**<br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_b.png)<br><br>
>> 
>>- **On constate que :**
>>   -    Les valeurs de $Dij0$ sont comprises entre **223** et **230** bits.<br>
>>      -    Les valeurs **224** et **225** sont les plus fréquentes, et elles rentrent dans la catégorie $\pm 1 bit$.<br>
>>      -    Les autres valeurs sont **_incertaines_** puisqu'en théorie $Dij0$ est **_constante_**.<br><br> 
>>- **Il faut maintenant répondre à la question :** Quelle est la valeur $DIJ0$<br>
>>   -  tel que $\forall$  $t_{I} \in [0,0001; t_{60s}]$  $Dij0(t_{I}) = DIJ0$ ?<br><br>
>></details>
>><details>
>>    <summary><b>Consolidation et courbe</b></summary><br>
>>
>>
>>- **Insérer la courbe suivante :** <br>
>>    - Abcisses : $time$
>>    - Ordonnées : $Gap$ (en bleu), $Gap\\_glis$ en rouge.<br><br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_c.png)<br><br>
>>
>>- **On constate que :**
>>   - La courbe correspondant au $Gap\\_glis$ montre l'effet de la moyenne glissante qui lisse ces dispersions
>>     sans pour autant masquer la forme en escalier de la courbe associée à $Gap$.<br>
>>
>>    - Dans les premières secondes du refroidissement les mesures sont aberrantes.
>>    - Si l'on fait un $Zoom$ sur ces premières secondes on obtient la courbe ci-dessous :<br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_cbis.png)<br><br> 
>>
>>- **On constate que :**
>>    -  La partie où  $t_{I} \in [0.0; 0.7]$ le TMP36 est encore tenu, pas de problème.
>>    -  La partie où  $t_{I} \in [0.7; 1.4]$ le TMP36 est en cours d'être relâché.
>>       Le TMP36 étant monté sur une platine de test sans soudure, tous mouvements perturbent les connexions.
>>    -  La partie où  $t_{I} \in [1.4, t_{60s}]$  le TMP36 n'est plus tenu, tout rentre dans l'ordre.<br>
>>
>>- **Il faut donc modifier la référence temporelle :**
>>   
>>    -  ( 1 ) Insérer une colonne entre les colonnes **[D]** et  **[E]**.
>>    -  ( 2 ) Affecter le nom $time_{d}$ à la colonne **[E]**
>>    -  ( 3 ) $\forall t_{I} \in [0.0001, t_{60s}]$  $E_{I} = D_{I} -  1.40023$ secondes <br>
>>    -  ( 4 ) Supprimer les lignes des mesures antérieures tel que $t_{I} \lt 1,40023$ seconde
>>    -  ( 5 ) Puisque les valeurs $Dij0 = 24$ sont les plus représentatives, nous imposons  $\forall t_{I}$ $\in [0.0000, t_{60s}]$  $B_{I} = 224$<br>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_d.png)<br><br> 
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
>>- ( 1 ) Ecrire respectivement dans les cellules ***H3, H4, H5*** les labels  $\tau$, $\epsilon$ , $Gap\\\_glis\\\_Max$ <br>
>>- ( 2 ) Nommer respectivement les cellules ***I3, I4, I5*** par __"Tau", "Epsilon" , "Gap_glis_Max"__ <br>
>>- ( 2bis ) Écrire respectivement les cellules ***I3, I4, I5*** les valeurs  __10, 1 , 24__ <br>
>>- ( 3 ) Donner à la cellule ***H8*** le titre _"model"_.
>>- ( 3bis ) Les cellules $H_{k}$ continnent la formule  $=Gap\\\_glis\\\_Max * EXP(- E_{k} / (Tau))$<br><br>
>>
>>- **Ci-dessous les formules, et le résultat de ces formules :**     
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_e.png)<br><br>
>>
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_ebis.png)<br><br>
>>
>></details>
>>
>><details>
>>    <summary><b>Ajout des calculs des moindres carrés</b></summary><br>
>>
>> Le principe est de minimiser la ***sommes des carrés des écart*** entre le _model_ et _la moyenne glissante_.<br>
>> C'est à dire minimiser l'erreur en faisant varier la valeur de $\tau$ :
>>
>> Pour simplifier la recherche manuelle de la valeur optimale de $\tau$ nous calculons l'effet de la variation de $\tau$ d'une valeur de $\pm\epsilon$:
>>- $K_{d} = model(time_{d}, \tau -\epsilon)) - Gap\\\_glis(time_{d})$
>>- $L_{d} = model(time_{d}, \tau )) - Gap\\\_glis(time_{d})$
>>- $M_{d} = model(time_{d}, \tau +\epsilon)) - Gap\\\_glis(time_{d})$
>>
>> $$erreur_{minimale} = \sum_{d=0}^{N} (model(time_{d}, \tau_{optimale})) - Gap\\\_glis(time_{d}))^{2}$$
>> 

>>- **Les formules à placer dans les cellules sont celles-ci :**
>>
>>- Pour $i \in [9, N]$<br>
>>
>>    -   Cellule **[Ki]** contient la formules Excel : $=(Gi-Gap\\_glis\\_Max*exp(-Ei/(Tau-Epsilon)))^2$
>>    -   Cellule **[Li]** contient la formules Excel : $=(Gi--Gap\\_glis\\_Max*exp(-Ei/Tau))^2$
>>    -   Cellule **[Mi]** contient la formules Excel : $=(Gi-Gap\\_glis\\_Max*exp(-Ei/(Tau+Epsilon)))^2$
>>
>>- Pour les cellules **[K7], [L7], [M7]**, on calcul la somme des carrés.<br>
>>
>>    -   Cellule **[K7]** contient la formules Excel : $=SOMME(K7:K_{N})$
>>    -   Cellule **[L7]** contient la formules Excel : $=SOMME(L7:L_{N})$
>>    -   Cellule **[M7]** contient la formules Excel : $=SOMME(M7:M_{N})$
>>
>>
>>- **De façon à aboutir à ceci du point de vue des formules :** 
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_f.png)<br><br>
>>
>>
>>- **De façon à aboutir à ceci du point de vue des résultats :** 
>>
>>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_fbis.png)<br><br>
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
>    - Le temps de mesure est de 60 secondes, prenons arbitrairement $\tau = 10 \pm5 secondes$<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_g1.png)<br><br>
>
>- Estimation suivante : 
>    - Le résultat précédent nous indique que $\tau_{optimal}$ est **> 15 s**. Prenons arbitrairement $\tau = 20 \pm4 s$<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_g2.png)<br><br>
>
>- Estimation suivante : 
>    -  Le résultat précédent nous indique que $\tau_{optimal}$ est **> 24 s**. Prenons arbitrairement $\tau = 25 \pm0.5 s$<br><br>
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_g3.png)<br><br>
>
>- Estimation suivante :
>    -  Le résultat précédent nous indique que $\tau_{optimal}$ est  **< 24,5 s**. Prenons arbitrairements $\tau = 23 \pm1 s$<br>
>       Et ainsi de suite jusqu'à ce que l'on estime que la précision est suffisante.<br><br>
> 
>![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Excel_cooling_step_2_g4.png)<br>
> 
>- Ainsi, on trouve que $\tau_{optimal}$ s'approche de la valeur $23 \pm0.5$ <br><br>
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
</details>
