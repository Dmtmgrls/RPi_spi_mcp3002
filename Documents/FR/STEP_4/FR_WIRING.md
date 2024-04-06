### NOUVEAU CÂBLAGE DES COMPOSANTS
Niveau ⭐⭐⭐⭐
<br><br>

- Par rapport au T.P. de niveau ⭐⭐⭐ :
    -  Le cablage est rigoureusement identique
    -  Par contre le code utilisera le mode $différentiel$ au lieu du mode $asymétrique$ (single ended).<br><br>
<details>
  <summary>MONTAGE A DEUX TMP36 DONT LEUR SORTIE EST RELIÉE A UN CANAL DIFFÉRENT</summary><br><br>

><details>
>  <summary>brochage des composants</summary><br>
>  
>> <details>
>>  <summary>TMP36</summary><br>
>>  
>> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/TMP36DZ_Pins.png)<br><br>
>> </details>
>>
>> <details>
>>  <summary>MCP3002</summary><br>
>>  
>> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_pins.png)<br><br>
>> </details>
>>
>> <details>
>>  <summary>Raspberry</summary><br>
>>  
>> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Raspberry_pin_GPIO.png)<br><br>
>> </details>
></details>
><details>
>  <summary>Schéma de montage</summary><br>
>  
>> ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Schema_montage_Etape_3_CH0_CH1_2_TMP36.png)<br><br> 
></details>
>
> <details>
>  <summary>Tableau des connexions</summary><br>
>  
>>| Broches<br> TMP16 on CH0| Broches<br> TMP16 on CH1| Broches <br>MCP3002 |  Broches<br>Pi 3B+ |
>>|------------------|-------------------|---------------------|--------------------|
>>|  :no_entry:       |  :no_entry:       |  1 (CS)             |  24 (CE0)          |
>>|  2 (Vout)         |   :no_entry:        |  2 (CH0)            | :no_entry:         |
>>|  :no_entry:        |   2 (Vout)        |  3 (CH1)            |  :no_entry:        |
>>|  3 (GND)          |   3 (GND)          | 4 (Vss)            |  25 (Masse)        |
>>|  :no_entry:        |  :no_entry:       |  5 (MOSI)           |  19                |
>>|  :no_entry:       |  :no_entry:       |  6 (MISO)           |  21                |
>>|  :no_entry:       |  :no_entry:       |  7 (CLK)            |  23                |
>>|  1 (+Vs)      |  1 (+Vs)       |  8 (VDD/Vref)       |  17 (<b>3.3V</b>)  |
></details>
>
</details>
