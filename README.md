# P22_ECU_Telemetry

ECU and Telemetry System for Prom Racing's P22 FSE Vehicle

![DSC_0186](https://user-images.githubusercontent.com/80124884/206901552-28a2b27d-7a98-4657-af19-d865792bd76d.jpg)

# Introduction
During my 3rd year of studies,I joined PROM Racing. After my first year with the team where I came to understand how an electric formula car is made,I finally had the chance to to design one myself. 

As an electrical and computer engineer with a focus on hardware systems and electronics the idea of designing and assembling a motorsports grade ECU was a very exciting project, and when the time came for me to finally start working on the project I was...very (you have no idea!)excited. 

# Conceptual Phase

Designing an electric vehicle and especially a race car is no simple task, and even though the design of PROM's previous car was well thought, it was still the team's first ever electric vehicle. Therefore, there was a good reference but there was still a long way to go!

Since the previous vehicle was based on a distributed model its ECU was extremely simple, and for me this was unacceptable!
My desire was to build an extremely robust and powerful ECU which would be the center of the car. 
And thus started a long process of carefully examining all my options and fine tuning the architecture of the vehicle's electronics

# The Final Concept

After months of planning and preperation I had finally come up with the final concept of the vehicle's architecture and consequently the ECUs main functionalities.
Instead of a distributed model a more centralized one would be used. At the center of it all we would have the vehicle's ECU. A mixture of analog(sensor)signals and digital signals would be sampled by the ECU and three different CAN Buses would be utilized for it to communicate with the other systems of the vehicle.

P22 General Datapath

![P22 General Datapath](https://user-images.githubusercontent.com/80124884/206898583-58e423ab-130f-4cac-8796-224570a0f653.png)

P22 ECU and Telemetry Datapath

![P22 ECU](https://user-images.githubusercontent.com/80124884/206898629-f3cebf54-8daf-428c-bc4a-8362b0870476.png)

# Brief Hardware Overview

ECU: An [STM32F407](https://www.st.com/resource/en/datasheet/dm00037051.pdf) was used as the main controller of the ECU. Though I would have prefered to use another MCU with more capabilities both in terms of processing power and peripherals I was not able to acquire one due to supply shortages. Thus, I had to resort to the aforementioned MCU. Even if the STM32F407 was not my first choice, with a clocking speed of 168 MHz, a dedicated FPU, 196 Kbytes of RAM, 3 ADCs and 2 CAN peripherals it was certainly more than capable to get the job done.

A fairly simple Power Supply scheme was used in order to convert the 12V  low voltage power supply of the vehicle to 3.3V. The power supply features reverse voltage protection, overcurrent protection, esd/tvs protection and overvoltage protection.

The signals sampled by the ECU, both digital and analog were buffered through dedicated hardware in order to protect and isolate the MCU from the inputs.

Telemetry: A seperate MCU was used for the Telemetry part. The decision to use a seperate MCU was based on the fact that there were several analog and digital signals (mainly from sensors)that were of no concern for the ECU. Thus, an [STM32F103](https://www.st.com/resource/en/datasheet/stm32f103rb.pdf) was used in order to relieve the ECU from unnecessary functionalities. 

Because there was a need for 2 CAN Buses, and the STM32F103 has only a single CAN peripheral, an [MCP2515](https://ww1.microchip.com/downloads/en/DeviceDoc/MCP2515-Stand-Alone-CAN-Controller-with-SPI-20001801J.pdf) stand-alone CAN Controller with SPI interface was used.

For the RF part of the ECU, an [NRF24L01](https://www.sparkfun.com/datasheets/Components/SMD/nRF24L01Pluss_Preliminary_Product_Specification_v1_0.pdf) was used, along with the [RFX2401C](https://www.skyworksinc.com/-/media/0CD3D8B5F6C74BF799F985E48EE2A072.pdf)


# SPI Drivers Development

The Telemetry part consists of a single SPI bus with 2 SPI slaves: the MCP2515 and the NRF24L01. Thus a long process of developing the drivers necessary for the telemetry part begun. 

The NRF24L01 datasheet was straightforward and with little effort I was able to get it up and running.

The MCP2515 was a different story however. The datasheet was a hard read and I was not able to continue without the help of a logic analyzer.

Thankfully, the [Saleae](https://www.saleae.com/) team had given to us the [Logic Pro 8](https://usd.saleae.com/products/saleae-logic-pro-8) as a sponsorship which was perfect for decoding the SPI Bus. However, I must admit that I was a bit disappointed that there was not an official CAN decoder available.

My setup

![20221211_122416](https://user-images.githubusercontent.com/80124884/206900327-efdb2ca8-d62e-4622-8598-cfe8a08b5db3.jpg)

![20221211_122428](https://user-images.githubusercontent.com/80124884/206900310-88d6b2a8-bd74-42ec-83ee-dc1812ddd3ab.jpg)

SPI Capturing

![Screenshot_10](https://user-images.githubusercontent.com/80124884/206900398-c1bbcab4-7145-465e-bef5-35ff36b29489.png)

![Screenshot_11](https://user-images.githubusercontent.com/80124884/206900410-a0394a50-6d46-4283-9494-271950b57a28.png)

With the help of the Logic Pro 8 I was able to understand what was going wrong and get my SPI to work properly. I concluded that there were some bit timing issues that occured due to a bug on the master side of the bus, which I was able to resolve with a bit of trial and error. This enabled me to continue my work with the help of my debugger and further develop the drivers for the MCP2515. Eventually, everything worked out and I was able to finally conclude the development of my firmware.

# RF Section

For the RF part, thanks to the NTUA ECE Electronics Lab I was able to get my hands on a spectrum analyzer and a VNA. This enabled me to fully develop my RF PCB and achieve the desired impedance matching, Rx Sensitivity and Tx Power. The only issue that I was not able to resolve was the Tx power of the 1st harmonic. Had I had more time to work on the RF section I would have been able to resolve it. However, given the nature of Formula Student you need to work hard and you need to work fast, and as such I decided to focus on the next task rather than seek perfectiom.

Spectrum Analyzer Capture

![Screen_0033](https://user-images.githubusercontent.com/80124884/206900707-e34fe321-e087-4694-8fcb-fd547dc928fd.png)

# Final Thoughts 

In hindsight there were a lot of  mistakes both in the overall design and the firmware, however they were not detrimental to the overall functionality of the ECU and Telemetry. 
The ECU and Telemetry project was a relative small part of my work but it was definitely a great learning experience. It was the first time I had to develop a schematic, the PCB, the firmware and assemble the PCB from ground zero and I am really proud that I was able to achieve all of the above in less than 2 months time.
