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
