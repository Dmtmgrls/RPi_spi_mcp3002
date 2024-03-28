###  CÂBLAGE DES COMPOSANTS
Niveau :star:
<details>
  <summary>brochage des composants</summary><br>
  
> <details>
>  <summary>TMP36</summary><br>
>  
> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/TMP36DZ_Pins.png)<br><br>
> </details>
>
> <details>
>  <summary>MCP3002</summary><br>
>  
> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/MCP3002_pins.png)<br><br>
> </details>
>
> <details>
>  <summary>Raspberry</summary><br>
>  
> ![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Raspberry_pin_GPIO.png)<br><br>
> </details>
</details>

<details>
  <summary>Schéma de montage</summary><br>
  
![Brochage](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Schema_montage_Etape_1.png)<br><br>
</details>

<details>
  <summary>Tableau des connexions</summary><br>
  
| Broches<br> TMP16 | Broches <br>MCP3002 |  Broches<br>Pi 3B+ |
|-------------------|---------------------|--------------------|
|  :no_entry:       |  1 (CS)             |  24 (CE0)          |
|  2 (Vout)         |  2 (CH0)            |  :no_entry:                |
|  :no_entry:       |  3 (CH1)            |  :no_entry:        |
|  3 (GND)          |  4 (Vss)            |  25 (Masse)        |
|  :no_entry:       |  5 (MOSI)           |  19                |
|  :no_entry:       |  6 (MISO)           |  21                |
|  :no_entry:       |  7 (CLK)            |  23                |
|  1 (VS+)          |  8 (VDD/Vref)       |  17 (<b>3.3V</b>)  |

</details>
