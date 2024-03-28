### ETUDE  DU MCP3002 NIVEAU :star:
<details>
    <summary>Etudions tout d'abord ce qu'est la numérisation d'une tension.</summary><br>

><details>
>    <summary>Définitions théorique de la numérisation d'une tension.</summary><br>
>
>>-  Soit $V_{Ref}$ la tension de référence appliquée à un convertisseur numérique à $N$ $bits$.<br>
>>   Alors par définition (il n'y a rien  à comprendre, c'est construit ainsi)<br><br>
>>
>>     - Du point de vu codage (grandeur dans $\mathbb{N}$)
>>        - Le convertisseur peut coder $2^N$ niveaux de tension différents.<br>
>>        - la valeur maximale que peut atteindre le code est $Code_{MAX} = 2^N-1$<br><br> 
>>
>>     - Du point de vu décodage (grandeur dans $\mathbb{R}$)  
>>        - Le pas du convertisseur est $p = \frac{V_{Ref}}{2^N}$ $[mV]$<br>
>>        - Au code $Code_{MAX}$ correspond la tension maximale $V_{MAX} = p * Code_{MAX} = V_{Ref}*\frac{2^N - 1}{2^N}$<br><br>
></details>
>
><details>
>    <summary>Appliquons ces définitions à notre circuit <b>MCP3002</b></summary><br>
>
>>   -  Il est alimenté sous $V_{DD} = V_{Ref} = 3300$ $[mV]$ , et $N = 10$<br><br>
>>       | Grandeur | Unité | | valeur en base 10 |valeur en base 16 |
>>       |---|---|---|---|---|
>>       | $N$ | $\subset \mathbb{N}$| |10 |   |
>>       | $2^N$ |$\subset \mathbb{N}$| | 1024 | 0x400 |
>>       | $Code_{min} | $\subset \mathbb{N}$| | 0 | 0x000 |
>>       | $Code_{MAX} |$\subset \mathbb{N}$ |$2^N - 1 $|  1023 | 0x3FF |
>>       | $p$ | $[mV]$| $\frac{3300}{1024}$ | 3,22...  | non représentable |
>>       | $V_{MAX}$ |$[mV]$| $3300*\frac{1023}{1024}$ | 3296,77... | non représentable |
></details> 
</details>

<details>
    <summary>Comment décoder la tension numérisée ?</summary><br>

> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_Decodage.png)
> 
>- Dans le shéma ci-dessus nous avons :<br><br>
>     -  A gauche le Convertisseur Analogique Numérique (C.A.N) qui en sortie, génère le $CODE\_{OUT}$<br>
>        correspondant au code représentant la tension d'entrée ${V_{in}}$.<br><br>
>     -  A droite une fonction qui converti $CODE\_{OUT}$ en une tension digitalisée $V_{digit}$ approchant la valeur de ${V_{in}}$.<br><br>
>         - Pourquoi dit on que $V_{digit}$ "**approche**" ${V_{in}}$ ?<br>
>             -  Tout simplement parce que $V_{digit}$ est un multiple du pas ${p}$ du C.A.N.<br>
>             -  Dit autrement, la tension $V_{digit}$ évolue par palier.
>             -  Elle n'est pas continue, mais discontinue.<br><br>
>
>- La fonction $F(Code_{OUT})$ est égale à $p * Code_{OUT}$ c'est à dire $V_{digit} = V_{Ref} * \frac{Code_{OUT}}{N}$
</details>

