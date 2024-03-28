## Chip Selection Guide
### Antenna
#### Ceramic patch antenna
- [CrossAir-C03](assets/RF_ANTENNA_CrossAir-C03_2.4GHZ.pdf) 2.4G
- [CrossAir-G01](assets/RF_ANTENNA_CrossAir-G01_GNSS.pdf)  GNSS
- [CrossAir-L01](assets/RF_ANTENNA_CrossAir-L01_LTE.pdf)  LTE
- [CrossAir-S01](assets/RF_ANTENNA_CrossAir-S01_NBIOT.pdf)  NBIOT

### LNA
- [AW5005](https://item.szlcsc.com/5725664.html) GNSS, 1.5V~3.6V, 18dB
- [BGA524N6](assets/RF_LNA_BGA524N6.PDF) GNSS, 1.5V~3.3V, 19.6dB

### PA
- [RFX2401C](https://item.szlcsc.com/19919.html) 2.4G, 22dB
- [AT2401C](https://item.szlcsc.com/838326.html) 2.4G, 22dB

### SAW
- [SAFFB1G56KB0F0A](https://item.szlcsc.com/92836.html) GNSS
- [SAFEA2G45MB0F0A](https://item.szlcsc.com/978988.html) 2.4G

## GNSS Antenna Design
### GNSS Frequency Bands and Signals
![alt text](<assets/GPS Anntena Design/image.png>)

- [Tune GPS Antena](assets/Tune%20GPS%20Antena.pdf)
- [How are GNSS Active Antennas Powered](assets/How%20are%20GNSS%20Active%20Antennas%20Powered.pdf)

## 2.4G Antenna Design
- [AN91445](https://www.infineon.com/dgdl/Infineon-AN91445_Antenna_Design_and_RF_Layout_Guidelines-ApplicationNotes-v09_00-CN.pdf?fileId=8ac78c8c7cdc391c017d073ded61621b) bible, original from `cypress` which adopted by `infineon`.
- [Antenna tuning](./assets/Antenna%20%20Tunning%20By%20Nordic.pdf) This white paper explains how to tune an antenna and subsequently improve radio communication range by `nordic`.