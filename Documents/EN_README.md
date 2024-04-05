# RPi_spi_MCP3002

>   - [README in french](https://github.com/Dmtmgrls/RPi_spi_mcp3002/blob/main/Documents/FR_README.md)

## In this document, always keep in mind the following:

```
We are just Dwarves sitting on the shoulders of Giants.
```
<br><br>

>[!NOTE]
**Keywords**:<br>
>TMP36, MCP3002, Raspberry, B3+, Zero 2W<br>
>sensor, temperature, converter, analog, digital, ADC<br>
>python, 3.9, linux, debian

<br>

## The objective of this document: To propose a pratical works using the SPI bus.
```
This practical work is divided into 5 stages of increasing difficulty but which can be easily assimilated.
The difficulty level is represented by the number of ⭐
Below we indicate the items addressed in each step.
Click on the small black arrow to expand the items.
```


<details>
   <summary><b><i>First step</i></b> : Get results quickly <b>THAT WORKS !!!</b> .<br></summary>

><br>
>
>- Indicate the necessary equipment.
>- Study the essential characteristics of each element.
>- Propose a minimal functional assembly comprising:
> - a single T° sensor
> - a single ADC
> - and the Raspberry obviously.
>- Offer a “turnkey” code that is often found on the WEB.
>- Quickly arrive at a first **It works!!!**<br>
> Which will make you want to go further.<br>
</details>

 <details>
    <summary><b><i>Second step</i></b> : Study of the SPI Bus, and the consequences for the MCP3002/Raspberry couple.<br></summary>

><br>
>
>- Understand how the $Bus$ $SPI$ works.<br>
>- Study the $spidev$ package
>- Study the communication between the MCP3002 and the Raspberry.<br>
>- Improved the $Turnkey$ python code from stage 1.<br>
</details>

<details>
    <summary><b><i>Third step</i></b> : Assembly with two T° sensors and one ADC in mode single-ended.<br></summary>
   
>- Propose a new functional assembly including:
>   - two T° sensors
>   - a single ADC
>- Analyze and compare the behavior of each element.
>- Understand the flaws of sampling.<br>

</details>

<details>
    <summary><b><i>Step four</i></b> : Assembly with two T° sensors and an ADC in pseudo-differential mode<br></summary>
   
>- Analyze and compare the behavior of level editing ⭐⭐⭐ in pseudo-differential mode.<br>

</details>

<details>
    <summary><b><i>Last stepe</i></b> : Assembly with three T° sensors and two ADC.<br></summary>

><br>
>
>- Propose a functional assembly using all the components:
> - three T° sensors
> - two ADC
>- Understand target addressing (CAN) by the initiator (Raspberry)
>- Observe the temperature measurements:
> - disparities between T° sensors on the same ADC
> - disparities between T° sensors on different ADC
>- Etc...
</details>

<br><br><br>

## Implementation:  :<br>

>[!IMPORTANT]
     Follow the order of the steps. Take the time to check. Take the time to understand

<details>
<summary>FIRST STEP. :star:<br></summary>
   
>- [Gather the materials](./FR/STEP_1/FR_HARDWARE.md)
>- [Component Wiring](./FR/STEP_1/FR_WIRING.md)
>- [Install the software environment](./FR/STEP_1/FR_SOFT.md)
>- [Tests and Observations](./FR/STEP_1/FR_TESTS.md)
</details>

<details>
<summary>SECOND STEP. :star::star:<br></summary>
   
>- [Study of the SPI bus](./FR/STEP_2/FR_STUDY_SPI.md)
>- [Study of the **spidev** pakage](./FR/STEP_2/FR_STUDY_SPIDEV.md)
>- [Study of ADC/Raspberry communication](./FR/STEP_2/FR_STUDY_COMUNICATION.md)
>- [Improved python code](./FR/STEP_2/FR_CODEFIX.md)
</details>

<details>
<summary>THIRD STEP. :star::star::star:<br></summary>

>- [Gather the materials](./FR/STEP_3/FR_HARDWARE.md)
>- [New component Wiring](./FR/STEP_3/FR_WIRING.md)
>- [Addressing Targets](./FR/STEP_3/FR_TARGET_ADDRESSING.md)
>- [Install the new program](./FR/STEP_3/FR_SOFT.md)
>- [Tests and Observations](./FR/STEP_3/FR_TESTS.md)
</details>

<details>
<summary>STEP FOUR. :star::star::star::star:<br></summary>

>- [Gather the materials](./FR/STEP_4/FR_HARDWARE.md)
>- [Component Wiring](./FR/STEP_4/FR_WIRING.md)
>- [Addressing Targets](./FR/STEP_4/FR_TARGET_ADDRESSING.md)
>- [Install the new program](./FR/STEP_4/FR_SOFT.md)
>- [Tests and Observations](./FR/STEP_4/FR_TESTS.md)
</details>

<details>
<summary>LAST STEP. :star::star::star::star::star:<br></summary>

>- [Gather the materials](./FR/STEP_5/FR_HARDWARE.md)
>- [Addressing Devices](./FR/STEP_5/FR_DEVICES_ADDRESSING.md)
>- [New component Wiring](./FR/STEP_5/FR_WIRING.md)
>- [Install the new program](./FR/STEP_5/FR_SOFT.md)
>- [Tests and Observations](./FR/STEP_5/FR_TESTS.md)
</details>

