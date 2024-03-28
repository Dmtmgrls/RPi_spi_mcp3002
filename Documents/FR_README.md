# RPi_spi_MCP3002

[README in english](./EN_README.md)

## Dans ce document, toujours garder à l'esprit ceci :

```
Nous ne sommes que des Nains assis sur les épaules de Géants.
```
<br><br>

>[!NOTE]
**Mots clé** :<br>
>TMP36, MCP3002, Raspberry, B3+, Zero 2W<br>
>capteur, température, convertisseur, analogique, numérique, CAN<br>
>python, 3.9, linux, debian

<br>

## L'objectif de ce document : Proposer un T.P. utilisant le bus SPI.
```
Ce TP est divisé en 4 étapes de difficultés croissantes mais assimilables facilement.
Le niveau de difficulté est réprésenté par le nombre d'⭐
Ci-dessous nous indiquons les items abordées dans chacune des étapes.
Cliquez sur la petite flèche noire pour développer les items.
```


<details>
   <summary><b><i>Première étape</i></b> : Arriver rapidement à un <b>CA MARCHE !!!</b> .<br></summary>

><br>
>
>- Indiquer le matériel nécessaire.
>- Etudier les caractéristiques essentielles de chaque élément.
>- Proposer un montage fonctionnel minimal comportant :
>    - un seul capteur de T°
>    - un seul CAN
>    - et le Raspberry évidemment.
>- Proposer un code clé en main que l'on trouve souvent sur le WEB.
>- Arriver rapidement à un premier **Ca marche !!!**<br>
>   Ce qui donnera envie d'aller plus loin.<br>
</details>

 <details>
    <summary><b><i>Seconde étape</i></b> : Etude du Bus SPI, et les conséquences pour le couple MCP3002/Raspberry.<br></summary>

><br>
>
>- Comprendre le fonctionnement du $Bus$ $SPI$.<br>
>- Etudier le package $spidev$
>- Etudier la communication entre le MCP3002 et le Raspberry.<br>
>- Amélioré le code python $CléEnMain$ de l'étatpe 1.<br>
</details>

<details>
    <summary><b><i>Troisième étape</i></b> :Montage à deux capteurs de T° et un seul CAN.<br></summary>
   
>- Proposer un nouveau montage fonctionnel comportant :
>   - deux capteurs de T°
>   - un seul CAN
>- Analyser et comparer le comportement de chaque élément.
>- Comprendre les défauts de l'échantillonage.<br>

</details>

<details>
    <summary><b><i>Dernière étape</i></b> : Montage à trois capteurs de T° et deux CAN.<br></summary>

><br>
>
>- Proposer un montage fonctionnel utilisant tous les composants  :
>    - trois capteurs de T°
>    - deux CAN
>- Comprendre l'adressage des cibles (CAN) par l'initiateur (Raspberry) 
>- Observer les mesures de température :
>    - disparités entre les capteurs de T° sur un même CAN
>    - disparités entre des capteurs de T° sur des CAN différents.
>- Etc ...
</details>

<br><br><br>

## Mise en oeuvre :  :<br>

>[!IMPORTANT]
    Respectez l'ordre des étapes. Prenez le temps de vérifier. Prenez le temps de comprendre

<details>
<summary>PREMIERE ETAPE. :star:<br></summary>
   
>- [Rassembler le matériel](./FR/STEP_1/FR_HARDWARE.md)
>- [Cablage des composants](./FR/STEP_1/FR_WIRING.md)
>- [Installer l'environnement logiciel](./FR/STEP_1/FR_SOFT.md)
>- [Tests et Observations](./FR/STEP_1/FR_TESTS.md)
</details>

<details>
<summary>SECONDE ETAPE. :star::star:<br></summary>
   
>- [Etude du bus SPI](./FR/STEP_2/FR_STUDY_SPI.md)
>- [Etude du pakage **spidev**](./FR/STEP_2/FR_STUDY_SPIDEV.md)
>- [Etude de la communication CAN/Raspberry](./FR/STEP_2/FR_STUDY_COMUNICATION.md)
>- [Amélioration du code](./FR/STEP_2/FR_CODEFIX.md)
</details>

<details>
<summary>TROISIEME ETAPE. :star::star::star:<br></summary>

>- [Rassembler le matériel](./FR/STEP_3/FR_HARDWARE.md)
>- [Nouveau cablage des composants](./FR/STEP_3/FR_WIRING.md)
>- [Addressage des Cibles](./FR/STEP_3/FR_TARGET_ADDRESSING.md)
>- [Installer le nouveau programme](./FR/STEP_3/FR_SOFT.md)
>- [Tests et Observations](./FR/STEP_3/FR_TESTS.md)
</details>

<details>
<summary>DERNIERE ETAPE. :star::star::star::star:<br></summary>

>- [Rassembler le matériel](./FR/STEP_4/FR_HARDWARE.md)
>- [Addressage des Devices](./FR/STEP_4/FR_DEVICES_ADDRESSING.md)
>- [Nouveau cablage des composants](./FR/STEP_4/FR_WIRING.md)
>- [Installer le nouveau programme](./FR/STEP_4/FR_SOFT.md)
>- [Tests et Observations](./FR/STEP_4/FR_TESTS.md)
</details>
