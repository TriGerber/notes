## UART

asynchronous, 2 fils tx/rx, sans horloge commune, vitesse définie: baud rate, bits début/fin

- Point à point, peu d'appareils
- très basique, 3 fils -> tx, rx, gnd
- plus lent que SPI

## SPI

MISO/MOSI (2 fils, un master -> slave, un slave -> master), SCLK (sysclk), SS/CS (chip-select, à 0 pour le device avec lequel on veut parler) synchronous

- Beaucoup d'appareils
- plus complexe, 4 fils par périphérique (clk, mosi, miso, cs)

## I2C

2 lignes partagées par tous + GND -> 3 fils. Chaque péri a une adresse et master utilise l'adresse pour sélectionner le slave à qui parler -> many slaves / many masters

- 3 fils -> plus complexe et lent derrière

-- 

RSSI, sensivity, SNR:
Pt = transmitter: 13dBm
Air noise: 60dB
Pr = receiver: -50dBm -> hearing noise at -50dB
amplifier (receiver + transmitter): 2dB + 2dB

13 + 2 + 2 - 60 = -43 -> 50 - 43 = +7 dB -> on reçoit.

link budget: au dessus de cb de dbm on peut aller -> how far can we reach
LTE 4G: 130 dBm
LoRa: 157 dBm
+20 dBm at 100mW constant RF output, 157 dB maximum link budget -> Max Pt power = 20 - (-137) = 157 dBm
High sensitivity: down to 137 dBm

Spreading factor: plus il est important, plus on est résistant au bruit. chaque fois qu'on augmente le spreading factor de 1, on double le nombr de bits et symboles. Higher SF = more bits per symbol =  higher transmission time = lower bitrate -> (bps=SF* (1/Tsymbol) = SF * BW/2^SF) Tsymbol = 2^SF / BW
SF12 -> 32'768 Tsymbol, 366 bps
SF7 -> 1'024 Tsymbol, 6836 bps



Bandwidth -> + BW/2 ou - BW/2 oscillations.
p.ex 860.1 MHz channel and 125 kHz BW -> 860'100'000 +- (125'000)/2

