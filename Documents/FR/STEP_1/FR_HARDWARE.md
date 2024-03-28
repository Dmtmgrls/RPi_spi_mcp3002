###  RASSEMBLER LE MATERIEL
Niveau :star:
<details>
  <summary>Le capteur de température TMP36.</summary><br>

>   <details>
>      <summary>Pourquoi ce modèle</summary>
>
>>-   Simple
>>-   Peu cher
>>-   Disponible chez les fournisseurs de composants électroniques pour le grand public.
>>-   Tension d'alimentation compatible avec le MCP3002 et le Raspberry.<br>
>   </details>
>
>   <details>
>      <summary>Son rôle</summary>
>
>>-   Il est de fournir un signal analogique dont l'amplitude dépend de la température (T°)
>>-   C'est ce signal qui sera appliqué à l'entrée du **C**onvertisseur **A**nalogique **N**umérique (C.A.N) <br>
>   </details>
>
>   <details>
>      <summary>Son brochage.</summary>
>
>-   Broche $V_{S}$ pour son alimentation
>-   Broche $GND$ reliée à la masse
>-   Broche $V_{OUT}$ est la la tension de sortie dont l'amplitude est proportionnelle à la température (T°).<br>
>    C'est cette broche qui sera reliée à l'entrée du **C**onvertisseur **A**nalogique **N**umérique (CAN).<br>
>
>    ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/TMP36DZ_Pins.png)
><br><br>
>   </details>
>
>   <details>
>      <summary>Ses caractéristiques.</summary>
>
>-  Essentielles et suffisantes à connaître pour ce T.P.
>     -   :point_right: Plage de tension d'alimentation  : $3.0  ≤ V_{S} ≤ 5.5$ Volts 
>     -   :point_right: Tension de sortie $V_{OUT}$ à  25°C : 750 mV
>     -   :point_right: Facteur d'échelle ou Sensibilité : 10mV/°C sur la plage -40°C à +125°C.<br><br>
>- Juste pour information
>     -   <i>Plage de tension de sortie : de 100mV à 2000mV></i>  
>     -   <i>Précision :
>        -   Qualité F à 25°C  entre -+1°C et +-2°C</i>
>        -   Qualité G à 25°C  entre -+2°C et  +-3°C   (qualité utilisée dans ce T.P.)</i> 
>     -   <i>Par curiosité vous pouvez télécharger la [Datasheet constructeur](https://www.analog.com/media/en/technical-documentation/data-sheets/TMP35_36_37.pdf)</i> 
>   </details>
</details>


<details>
  <summary>Le C.A.N MCP3002.</summary><br>
  
>   <details>
>      <summary>Pourquoi ce modèle</summary>
>
>>-   Simple
>>-   Disponible en format <i>8 broches Plastique Dual in-Line PDIP</i><br>
>>    Donc peu cher.
>>-   Disponible chez les fournisseurs de composants électroniques pour le grand public.
>>-   Tension d'alimentation compatible avec le TMP36 et le Raspberry.
>>-   Communique par bus **SPI** en tant que **cible**.<br><br>
>   </details>
>
>   <details>
>      <summary>Son rôle</summary>
>
>>-   Reçois, en entrée, le signal analogique issu du capteur de température **TMP36**.
>>-   Numérise ce signal analogique.
>>-   **Ne** transmet le résultat de la numérisation **que** sur demande du Raspberry.
>>    -  Pour ce T.P. et du point de vue SPI :<br>
>>        -   Le Raspberry sera toujours l'**initiateur** de la communication.<br>
>>        -   Tant dit que le C.A.N. MCP3002 sera toujours la **cible** du Raspberry.<br><br>
>>      
>>        -  :exclamation: sur le bus SPI, les **cibles** ne sont jamais à l'origine de la transmission de données. 
>>        -  :exclamation: Le MCP3002 ne fait que répondre à une demande de transmission de données de la part du Raspberry.<br>
>   </details>
>
>   <details>
>      <summary>Son brochage.</summary><br>
>
>>>- Les termes anglais désignant les composants compatibles SPI sont les suivants :
>>>    -  **Device** : C'est le composant lui même.
>>>    -  **Chanel** : C'est l'entrée analogique du Device, ou encore le canal d'entrée du Device.<br>
>>>- Pour référencer une entrée analogique **cible** on utilise le couple **Device/Chanel**<br>
>>>  En effet, un bus SPI peut comporter plusieurs **Devices** comportant eux mêmes plusieurs **Chanels**<br>
>>>  C'est la cas du MCP3002 qui est bi-canal<br><br>
>>> **Brochage vue de desssus** <br><br>
>>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_pins.png)<br><br>
>>>
>>> La broche non utilisée au niveau :star: sera notées :no_entry:<br>
>>>
>>> | Broche | Nom     | Rôle |
>>> |--------------|-----------|------------|
>>> | 1 | Chip Select/Shutdown      | Sélection du composant  (Device)      |
>>> | 2 | CH0 | Entrée analogique, Canal 0.<br>La sortie analogique du **TMP36** sera connectée à cette broche|
>>> | 3 :no_entry:| CH1 | Entrée analogique, Canal 1      |  
>>> | 4 | $V_{SS}$  | Masse       | 
>>> | 5 | Data in (MOSI)       | Entrée des demandes issues de l'initiateur       |
>>> | 6 | Data out (MISO) | Sortie des données vers l'initiateur       |
>>> | 7 | CLK | Signal d'horloge  (synchronisation)     |  
>>> | 8 | $V_{DD}$ ou $V_{REF}$  | Respectivement, Tension alimentation et tension de référence       |   
>
>   </details>
>
>   <details>
>      <summary>Ses caractéristiques.</summary>
>
>>-  Essentielles et suffisantes à connaître pour cette première étape du T.P.
>>    -  :point_right: Plage de tension d'alimentation $V_{DD}$ : $2.7 ≤ V_{S} ≤ 5.5$ $[V]$ .
>>    -  :point_right: Tension applicable sur les entrées analogiques : entre $V_{SS}$ (masse) et $V_{DD}$.
>>    -  :point_right: C.A.N. à bus SPI.
>>    -  :point_right: Deux entrées annalogiques.
>>    -  :point_right: Résolution : 10 bits soit 2<sup>10</sup> = 1024 echantillons distincts.<br><br>
>>         - le plus petit échantillon binaire (Less Sample Bit) représentera une tension d'entrée  égale à $\frac{V_{REF}}{1024}$ mV.<br>
>>           :bomb: C'est une division entière, il y aura donc toujours une erreur inférieure à $\frac{V_{REF}}{1024}$ mV.<br><br>
>>         - le code binaire codant la tension analogique d'entrée $V_{IN}$ sera égale à $1024 *\frac{V_{IN}}{V_{REF}}$.<br>
>>           :bomb: C'est une division entière, il y aura toujours une erreur de 1 bit sur le bit de poids faible.<br><br>
>>           Exemple :
>>              - $V_{IN} = 500$  mv,  $V_{DD} = V_{REF} = 3300$ mv donne $1024 *\frac{500}{3300} = 155,151.. > 155 = 0x09E = 0b00.1001.1110$ 
>>              - $V_{IN} = 2500$ mv, $V_{DD} = V_{REF} = 3300$ mv donne $1024 *\frac{2500}{3300}  = 775,75.. > 775 = 0x307 = 0b11.0000.0111$   
>>   
>>- Juste pour information
>>    -  <i>Echantillonnage par seconde à VDD 2,7 V : 75 Ksps</i>  
>>    -  <i>Par curiosité vous pouvez télécharger la [Datasheet constructeur](https://ww1.microchip.com/downloads/aemDocuments/documents/APID/ProductDocuments/DataSheets/21294E.pdf)</i> 
>   </details>
</details>

<details>
  <summary>Le Raspberry 3B+.</summary><br>
  
>   <details>
>      <summary>Pourquoi ce modèle</summary>
>
>>-   Il est deux à trois fois plus cher qu'un Pi Zero 2W mais il a plus de possibilités de connexion USB, LAN, HDMI.
>>-   Il a une interface **Wifi** et **Lan** ce qui limite les connexions entre le Pi B3+ et le PC.
>>-   Utilise le langage python qui est un langage interprété et non un langage compilé comme C/C++
>>-   Disponible chez les fournisseurs de composants électroniques pour le grand public.
>>-   Tension d'alimentation compatible le TMP36, MPC3002.
>>-   Il possède de nombreuses interfaces de communication. En particulier le bus **SPI**
>>-   Sa mise en oeuvre est lagement expliquée par le constructeur : [Getting- started](https://www.raspberrypi.com/documentation/computers/getting-started.html)<br><br>
>   </details>
>
>   <details>
>      <summary>Son rôle</summary>
>
>>-   Il gére les relevés de température (acquisition de données, présentation des données).<br>
>   </details>
>
>   <details>
>      <summary>Le brochage de son bus <b>SPI</b>.</summary><br>
>
>>>- Brochage vu haut dessus de la platine du Pi 3B+.<br><br>
>>> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Raspberry_pin_GPIO.png)<br><br>
>>>
>>> | Broche |  Nom      | Rôle |
>>> |--------|----------|------|
>>> | 17 | 3V3 power   | Alimentation du MCP3002 et TMP36       |
>>> | 25 | Masse  | Masse des MCP3002 et TMP36    |    
>>> | 26 | CE1 | Sélecteur du Device 1     |
>>> | 24 | CE0        | Sélecteur du Device 0       |
>>> | 21 | MISO |    Dialogue depuis la **cible** vers le Raspberry      |
>>> | 19 | MOSI | Dialogue depuis le Raspberry vers la **cible** |
>>> | 23 |  SLCK  | Horloge de synchronisation       | 
>
>   </details>
>
>   <details>
>      <summary>Les caractéristiques de son bus SPI.</summary>
>
>>-  Essentielles et suffisantes à connaître pour cette première étape du T.P.
>>    -  :point_right: Bien que la fréquence d'horloge puisse atteindre théoriquement les 35MHz, elle se limite dans les faits à <b>4000/4800 Hz</b>.<br><br>
>>    -  :point_right: Le Raspberry peut adresser deux **Devices** quelque soient le nombre de leurs cannaux<br> 
>>        -   Les <b>Devices</b> sont sélectionnés par hadrware. C'est à dire, par les broches <b>25</b> et<b>24</b> du Bornier.<br>
>>            Un seul <b>Devices</b> peut être sélectionné à un instant donné.<br>
>>        -   Les <b>Cannaux</b> sont sélectionnés par le software. Là aussi, un seul canal peut être sélectionné à un instant donné.
>>   
>>- Juste pour information
>>    -  <i>Par curiosité vous pouvez télécharger la [Datasheet constructeur](https://ww1.microchip.com/downloads/aemDocuments/documents/APID/ProductDocuments/DataSheets/21294E.pdf)</i> 
>   </details>
</details>

<details>
      <summary>Divers.</summary>
  
>-  Une **plaque de montage rapide** d'au moins 400 contacts<br>
>  ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Plaque_montage_rapide.png)<br><br>  
>-  Une vingtaine de **Câble de connexion Mâle/Femelle** de toutes couleurs.<br>
>  ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Cable_connexion_MF.png)<br><br>  
</details>



