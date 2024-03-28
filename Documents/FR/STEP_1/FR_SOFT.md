### ENVIRONNEMENT LOGICIEL
Niveau :star:

<details>
  <summary>Création de l'environnement de travail sur le Raspberry <b>Pi 3B+</b></summary><br>
  
|:information_source: | Il est conseillé de créer le même environnement de travail que celui indiqué dans ce TP |
|---|---|

> <details>
>     <summary>Création des répertoires</summary><br>
>
>- Les commandes ci dessous vont :<br>
>     - Désactiver un éventuel environement virtuel actif.<br>
>     - Définir dans la varaible <b>PATH_WORK</b> le nouveau PATH complet du répertoire de travail.<br>
>     - Créer le répertoire de travail , et si tout va bien se positionner dans celui-ci.<br><br> 
>
>``` shell
> cd 
> deactivate 2>/dev/null
> # PATH_WORK="Developpement_Python/Temperature/RPi_spi_mcp3002"  
> PATH_WORK="Level_1/Level_2/Level_3"
> rm -r $PATH_WORK 2>/dev/null
> mkdir -p $PATH_WORK && cd $PATH_WORK 2>/dev/null
> if [ $? -eq 0 ] ; then
>     clear; printf "\n\nOK Repertoire de travail cree.\n   Passer a la creation de l'Environnement Virtuel.\n\n"
> else
>     clear; printf "\n\nATTENTION repertoire de travail NON CREE !!\n\n"
> fi
>```
> </details>
>
> <details>
>     <summary>Création d'un environnement virtuel.</summary><br>
>
>   |  Explication Niveau  :star::star::star: la notion d'environnement virtuel  | [ℹ️](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment) |
>   |---|---|
>
> 
>- Les commandes ci dessous vont :<br>
>     - Créer dans le répertoire de travail un nouveau répertoir <b>env</b>.<br>
>     - Le répertoire de travail va devenir en un environnement virtuel.<br>
>     - Tout ce qui sera installé, sera installé sous <b>env</b> et non pas dans l'OS du Rasberry.<br>
>        Cela permet de ne pas polluer l'OS par nos essais.<br>
>        Plus tard il suffira de supprimer cet environnement, et tout sera oublié <br>
>        du point de vu de l'OS du Raspberry<br><br> 
>
>``` shell
> cd 
> deactivate 2>/dev/null
> # PATH_WORK="Developpement_Python/Temperature/RPi_spi_mcp3002"  
> PATH_WORK="Level_1/Level_2/Level_3"
> clear; cd ; cd $PATH_WORK 2>/dev/null
> if [ $? -eq 0 ] ; then 
>    clear ; printf "\nPATIENTEZ .....\n\n"
>     python3  -m venv env 
>     clear
>     source env/bin/activate
>     if [ $? -eq 0 ] ; then 
>          printf  "\n\nOK : Environnement Virtuel cree\n\n"
>     else
>          printf  "\n\nATTENTION :  ENVIRONNEMENT VIRTUEL NON CREE\n\n"
>     fi
> else
>     printf  "\n\nATTENTION : Le répertoire $PATH_WORK doit etre cree avant de creer l'environnement virtuel.\n\n"
> fi 
>```
>
>
>
> </details>
>
>
> <details>
>     <summary>Installation du package <b>spidev</b>.</summary><br>
> 
>- Ce package permet au <b>RPi 3B</b> de gérer le bus <b>SPI</b> :<br>
>     - Le parametrage des <b>Canaux</b>
>     - La sélection des couples <b><Device/Canal></b>
>
> ```shell
>  pip3 install "spidev>=3.6";  
> ```
> </details>
>
</details>

<details>
     <summary>Récupération du programme <b>"step_1_read_SPI.py"</b> de lecture de la température.</summary><br>
  
>
> | Commande | Commentaire |
> |----------|-------------|
> | **cd**<br>**cd** Developpement_Python/Temperature/RPi_spi_mcp3002  | Se positionner dans le répertoir de travail|
> | **touch** step_1_read_SPI.py | Création du fichier **step_1_read_SPI.py** vide<br>Conserver le même nom de fichier que celui du T.P. |
> | **vi** step_1_read_SPI.py<br>ou<br>**nano** step_1_read_SPI.py    |ouvrir le fichier dans un éditeur de texte de votre choix. |
> | ![](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/PICTURES/Soft_Copier_code.png)| Copier dans GitHub [ce code ci :star:](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/step_1_read_SPI.py). |>
> | | Coller ce code dans l'éditeur de texte.|  
> |  |Enregistrer le fichier et sortir de l'éditeur de texte. |
> | **ls -l** step_1_read_SPI.py |vérifier la présence du fichier et sa taille. | >
> 

</details>

<details>
     <summary>Activation du <b>Bus SPI</b> du Raspberrye.</summary><br>

>- Cela a été expliqué mille fois sur le **WEB** donc se reporter à ce document : :link: [Activer le bus **SPI**](https://www.raspberryme.com/activer-linterface-spi-sur-le-raspberry-pi/)<br><br>
>
>-  En cas d'**erreur 404** :shit: le principe est le suivant :
>  
>  | Ce qu'il faut faire<br>. | :pencil: Linux  / :point_up_2: Choix<br> .|
>  |--------|-------------|
>  | Se placer dans le répertoire de travail et lancer la commande<br>Une fenêtre s'ouvre | **sudo raspi-config** :pencil:|
>  | Dans la fenêtre sélectionner l'item.<br>Pour se déplacer utiliser les flèches :arrow_down: :arrow_up:| **Interfacing option** :point_up_2: |
>  | Une nouvelle fenêtre s'ouvre, sélectionner SPI | **SPI** :point_up_2:|
>  | Mettre en surbrillance en utilisant la touche **[TAB]**.| **Select** :point_up_2:|
>  | Une nouvelle fenêtre s'ouvre "<i>Would you like the SPI interface to be enabled ?</i>" | **Yes** :point_up_2:|
>  | Une nouvelle fenêtre s'ouvre "<i>The SPI interface is enabled</i>"| **OK** :point_up_2:|
>  | Une nouvelle fenêtre s'ouvre "<i>Would you like reboot now ?</i>" | **Yes** :point_up_2:|
>  | C'est terminé. Le Raspberry reboot .. Se reconnecter. | ... |

</details>

<details>
  <summary>A TRIER</summary><br>
  
````
https://github.com/ArcadiaLabs/Adafruit-raspi-python/blob/master/Adafruit_MCP3002/MCP3002.py
````

````
def read(adc_channel=0, spi_channel=0):
    spi = spidev.SpiDev()
    spi.open(0, spi_channel)
    spi.max_speed_hz = 1200000 # 1.2 MHz
    cmd = 128
    if adc_channel:
        cmd += 32
    reply_bytes = spi.xfer2([cmd, 0])
    reply_bitstring = ''.join(bitstring(n) for n in reply_bytes)
    reply = reply_bitstring[5:15]
    spi.close()
    return int(reply, 2) / 2**10
````


````
https://raspberrypi.stackexchange.com/questions/84736/reading-values-using-mcp3002

# botbook_mcp3002.py - read analog values from mcp3002
# (c) BotBook.com - Karvinen, Karvinen, Valtokari

# Installing spidev: 
#  sudo apt-get update
#  sudo apt-get -y install git python-dev
#  git clone https://github.com/doceme/py-spidev.git
#  cd py-spidev/
#  sudo python setup.py install

import spidev   # installation help in botbook_mcp3002.py comments
import time

def readAnalog(device = 0,channel = 0):
    assert device in (1, 0)
    assert channel in (1, 0)
    #open spi
    spi = spidev.SpiDev()
    #spi.open(0, device)
    spi.open(0, device)
    """
    Protocol start bit (S), sql/diff (D), odd/sign (C), MSBF (M)
    Use leading zero for more stable clock cycle
    0000 000S DCM0 0000 0000 0000
    Sending 3 8bit packages so xpi.xfer2 will return the same amount.
    start bit = 1
    sql/diff = 1 SINGLE ENDED MODE  (2 channel mode) 
    odd/sign = channel 0/1
    MSBF = 0
    """
    command = [1, (2 + channel) << 6, 0]
    #2 + channel shifted 6 to left
    #10 or 11 << 6 = 1000 0000 or 1100 0000
    reply = spi.xfer2(command)

    """
    Parse right bits from 24 bit package (3*8bit)
    We need only data from last 2 bytes.
    And there we can discard last two bits to get 10 bit value 
    as MCP3002 resolution is 10bits
    Discard reply[0] byte and start from reply[1] where our data starts
    """ 
    value = reply[1] & 31   
    #31 = 0001 1111 with & operation makes sure that we have all data from XXXX DDDD and nothing more. 0001 is for signed in next operation.
    value = value << 6  #Move to left to make room for next piece of data.
    #000D DDDD << 6 = 0DDD DD00 0000
    #Now we get the last of data from reply[2]
    value = value + (reply[2] >> 2)
    #Here we discard last to bits
    #DDDD DDXXX >> 2 = 00DD DDDD
    #0DDD DD00 0000 + 00DD DDDD = 0DDD DDDD DDDD
    spi.close()
    return value


def main():
    #read channel 0 on device 0
    value = readAnalog(0, 0)
    print(value)
    #time.sleep(10)

if __name__ == "__main__":
    main()
````
</details>
