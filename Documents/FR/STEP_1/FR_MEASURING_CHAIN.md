### CHAINE DE MESURE

-   Nous avons vu précédemment lors de l'étude du TMP36 que la tension de sa sortie $V_{OUT}$ était une fonction linéraire de la température.<br>
    Nous avons pu établir l'équation de cette fonction $f(V_{OUT}) \longrightarrow  \theta°$.<br><br>
    
    Que peut faire le Raspberry à ce niveau ? :
    -  Ne sachant pas lire la grandeur Analogique $V_{OUT}$ il est bloqué, sauf à lui fournir la conversion $V_{OUT_{TMP36}} \longrightarrow V_{Digitalised}$<br><br>

-  C'est la raison pour laquelle nous insérons le C.A.N MCP3002 entre le TMP36 et le Raspberry<br>
   Lors de l'étude du MDP3002 nous avons vu qu'il réalise la  conversion nécessaire $V_{OUT_{TMP36}} \longrightarrow CODE_{Numérique}$.<br>
   De plus, nous avons établi l'équation de la fonction qui réalise la conversion $F(CODE_{Numérique}) \longrightarrow V_{Digitalised}$

   Que peut faire le Rasberry à ce niveau ? :
   -  Il peut lire la grandeur Numérique $CODE_{Numérique}$
   -  Il peut faire le calcul de la fonction  $F(CODE_{Numérique}) \longrightarrow V_{Digitalised}$
   -  Il peut faire le calcul de la fonction  $f(V_{Digitalised})  \longrightarrow  \theta°_{Digitalised}$<br><br>

 - Le schéma de principe ci-dessous résume tout ce qui a été expliqué jusqu'à présent.<br>
 - Les deux fonctions sont :
      -  $f(V_{Digitalized}) = \theta_{Digitalized} = \frac{V_{Digitalized} - Offset}{Sensibility}$ en $[°C]$<br><br>
      -  $F(Code_{Numerique}) = V_{Digitalized} = V_{Ref} * \frac{Code_{Numerique}}{2^N}$ en $[mV]$

   ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Chaine_de_Mesures.png)

> [!IMPORTANT]
> Si certains points restent encore obscures, ne pas hésiter à revenir sur les explications précédentes avec ce shéma en tête.

