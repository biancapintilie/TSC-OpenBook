# OpenBook

## Descriere
OpenBook este un dispozitiv portabil bazat pe microcontrollerul ESP32-C6, proiectat pas cu pas, incepand cu schema componentelor, apoi schema electrica si in final modelul 3D al acestuia, urmarind schemele si indrumarile oferite in enuntul temei.
Pentru schematic am descarcat libraria din enunt si am folosit piesele de acolo pentru a realiza componentele si le-am asezat in frame exact ca in exemplul din enunt.
La partea de PCB am amplasat piesele dupa desenul dat, urmand masuratorile exacte, iar rutarea am facut-o folosind combinatia intre autorute si rute manuale pentru evitarea erorilor, fiind nevoita sa mai misc piesele incat sa pot ruta cat mai ok. Apoi am facut planurile de masa, atat pentru top cat si pentru bottom.
In final, am cautat modelul 3D pentru fiecare piesa pentru a-l adauga la forma 3D finala a dispozitivului, impreuna cu carcasa acestuia.

## Hardware

### **1. Microcontroller U2 – ESP32-C6-WROOM-1-N8**
#### - Modul principal de procesare, cu:
 - Procesor RISC-V single-core, 160 MHz
 - 	Wi-Fi 6 și Bluetooth 5.0
 - 	8 MB flash
 - 	Alimentare: 3.3V
#### - Conexiuni principale:
- SPI (pentru e-paper și microSD)
- I2C (pentru RTC și senzori)
- GPIO (pentru butoane, LED-uri, control power)

#### - Pini folosiți ESP32-C6:
| Semnal | Pin ESP32	| Funcție |
|--------|------------|---------|
| MOSI | GPIO10 |	Date spre e-paper |
| SCK | GPIO9 |	Ceas SPI |
| CS | GPIO8 | Chip Select |
| DC | GPIO7 | Data/Command |
| RST | GPIO5 |	Reset display |
| BUSY | GPIO6 | Status afișaj |


### 2. Afișaj e-paper J1 – FH34SRJ-24S-0.5SH
- Afișaj conectat prin SPI
- Condensatori de filtrare în jurul e-paper:
- EPD_C1–C12: 0.1–1uF (filtrare alimentare și stabilizare semnal)
- MOSFET: Q3 – SI1308EDL (pentru comutarea alimentării e-paperului)
  

### 3. Memorie și stocare U1 – W25Q512JVEIQ
- 64MB NOR Flash SPI
- Pentru stocarea fișierelor locale, fonturi, etc.
microSD (SPI shared):
- Partajat cu SPI-ul e-paper
- CS separat (ex: GPIO15)


### 4. Alimentare & management baterie Li-Po 3.7V
#### - Încărcare:
- MCP73831 (U MCP73831) – încărcare baterie prin USB
- Diodă protecție: D3–D5 (MBR0530)
#### - Monitorizare baterie:
- MAX17048 (U4) – Fuel Gauge via I2C
- Divizor rezistiv: R1_BAT, R2_BAT
- ADC ESP32: GPIO1
#### - Condensatori alimentare:
- C1_BAT, C2_BAT, C3, etc. – diverse capacități pentru filtrare și stabilizare (4.7uF – 100uF)


### 5. Ceas în timp real (RTC) U3 – DS3231SN
- RTC cu I2C, extrem de precis
- Backup pe supercapacitor (C10_SUPERCAP)
- Conexiune I2C: GPIO21 (SDA), GPIO22 (SCL)


### 6. Senzor SENSOR2 – BME680
- Senzor de mediu: temperatură, presiune, umiditate, COV
- Interfață I2C (partajat cu RTC)


### 7. Interfață utilizator
#### - Butoane fizice:
- BOOT_BUTTON, RESET_BUTTON, CHANGE_BUTTON
- Conectate pe GPIO2, GPIO3, GPIO4 (cu pull-up R_RESET, R_CHANGE etc.)
#### - LED RGB pentru feedback:
- CHG_LED – conectat la GPIO18 (PWM)


### 8. Protecție și filtrare
- D1 – USBLC6-2SC6Y – protecție ESD pe linia USB
- PGB1010603MR (D6, D8–D12) – protecție la supratensiune
- PFMF.050.1 – Varistor pentru liniile de alimentare


### 9. Alte circuite pasive
- Filtru LC pe alimentare display: L1 – 68uH, C7, C8
- Diverse rezistențe pull-up/down, limitare curent, etc.:
- R1–R10, R_BOOT, R_PWRUSB, R_CAPACITOR etc.
- Majoritatea în format 0402


### 10. Test Points & Debug
- TP1–TP17: puncte de test pentru semnale critice
- UART debug: GPIO43 (TX), GPIO44 (RX) – pentru încărcare firmware și debug

### Carcasă și design fizic
- PCB în format compact, optimizat pentru portabilitate
- Carcasă printată 3D
- Acces facil la conectorul USB-C (J2) pentru încărcare
  

## Schema Bloc
![image](https://github.com/user-attachments/assets/fa522e8e-549b-4be4-9a1b-7f61022e6712)

## Bill Of Materials

| Componenta | Link | Datasheet |
|-----------|--------------|-----------|
| Capacitor 0402 | [Model](https://componentsearchengine.com/part-view/CC0402MRX5R5BB106/YAGEO) | [Datasheet](https://componentsearchengine.com/Datasheets/2/CC0402MRX5R5BB106.pdf) |
| CPH3225A | [Model](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/CPH3225A/Seiko+Instruments/view-part/?ref=eda) |
| RCL 3528 | [Model](https://www.snapeda.com/parts/TAJB475K025RNJ/AVX/view-part/?ref=dk&t=capacitor%203528&con_ref=None) | [Datasheet](https://s3.amazonaws.com/snapeda/datasheet/TAJB475K025RNJ_AVX.pdf) |
| Resistor 0402 | [Model](https://componentsearchengine.com/part-view/R0402%201%25%20100%20K%20(RC0402FR-07100KL)/YAGEO) | [Datasheet](https://www.yageo.com/upload/media/product/products/datasheet/rchip/PYu-RC_Group_51_RoHS_L_12.pdf) |
| Dioda MBR0530 | [Model](https://eu.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/schottky-3165252.pdf) |
| Dioda PGB1010603MR | [Model](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/PGB1010603MR/Littelfuse%20Inc./datasheet/) |
| Buton | [Model](https://industry.panasonic.com/global/en/products/control/switch/light-touch/number/evqpuj02k) | [Datasheet](https://industry.panasonic.com/global/en/downloads?tab=catalog&small_g_cd=203&part_no=EVQPUJ02K) |
| LED 0603 | [Model](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/view-part/?ref=search&t=LED%200603) | [Datasheet](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/datasheet/) |
| Sensor BME680 | [Model](https://www.digikey.ro/en/models/7401317) | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme680-ds001.pdf) |
| SJ | [Model](https://grabcad.com/library/solder-jumpers-1) | [Datasheet](https://grabcad.com/library/solder-jumpers-1) |
| ESP32C6 | [Model](https://ro.mouser.com/ProductDetail/EPCOS-TDK/B72520T0350K062?qs=dEfas%2FXlABIszF52uu7vrg%3D%3D) | [Datasheet](https://www.snapeda.com/parts/RC0603JR-070RL/Yageo/datasheet/) |
| FH34SRJ-24S-0.5SH (J1) | [Model](https://www.snapeda.com/parts/FH34SRJ-24S-0.5SH(99)/Hirose/view-part/) | [Datasheet](https://www.snapeda.com/parts/FH34SRJ-24S-0.5SH(99)/Hirose%20Connector/datasheet/) |
| USB4110-GF-A (J2) | [Model](https://componentsearchengine.com/part-view/USB4110-GF-A/GCT%20(GLOBAL%20CONNECTOR%20TECHNOLOGY)) | [Datasheet](https://gct.co/files/drawings/usb4110.pdf) |
| QWIIC Connector (J3) | [Model](https://www.snapeda.com/parts/PRT-14417/SparkFun/view-part/) | [Datasheet](https://www.snapeda.com/parts/PRT-14417/SparkFun%20Electronics/datasheet/) |
| 112A-TAAR-R03 (J4)| [Model](https://www.snapeda.com/parts/112A-TAAR-R03/Attend/view-part/) | [Datasheet](https://www.snapeda.com/parts/112A-TAAR-R03/Attend/datasheet/) |
| 744043680 (L1)| [Model](https://ro.mouser.com/ProductDetail/Wurth-Elektronik/744043680?qs=PGXP4M47uW6VkZq%252BkzjrHA%3D%3D) | [Datasheet](https://www.we-online.com/components/products/datasheet/744043680.pdf) |
| DMG2305UX-7 (Q1, Q2) | [Model](https://componentsearchengine.com/part-view/DMG2305UX-7/Diodes%20Incorporated) | [Datasheet](https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay%20Siliconix/datasheet/) |
| SI1308EDL-T1-GE3 (Q3) | [Model](https://componentsearchengine.com/part-view/SI1308EDL-T1-GE3/Vishay) | [Datasheet](https://www.snapeda.com/parts/SI1308EDL-T1-GE3/Vishay%20Siliconix/datasheet/) |
| W25Q512JVEIQ (U1) | [Model](https://www.snapeda.com/parts/W25Q512JVEIQ/Winbond+Electronics/view-part/?ref=eda) | [Datasheet](https://www.winbond.com/resource-files/W25Q512JV%20SPI%20RevB%2006252019%20KMS.pdf) |
| ESP32C6 WROOM-1-N8 (U2) | [Model](https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif+Systems/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/ESP32-C6-WROOM-1-N8/Espressif+Systems/view-part/?ref=eda) |
| DS3231SN (U3) | [Model](https://www.snapeda.com/parts/DS3231SN%23/Analog+Devices/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/DS3231SN%23/Analog%20Devices/datasheet/) |
| MAX17048G+T10 (U4) | [Model](https://www.snapeda.com/parts/MAX17048G+T10/Analog+Devices/view-part/?ref=eda) | [Datasheet](https://www.snapeda.com/parts/MAX17048G+T10/Analog%20Devices/datasheet/) |
| MCP73831 Power Management (U5) | [Model](https://www.snapeda.com/parts/MCP73831T-2ACI/OT/Microchip/view-part/) | [Datasheet](https://www.snapeda.com/parts/MCP73831T-2ACI/OT/Microchip/datasheet/) |
| BD5229G-TR (IC1) | [Model](https://www.digikey.ee/en/models/658502) | [Datasheet](https://fscdn.rohm.com/en/products/databook/datasheet/ic/power/voltage_detector/bd52xxg-e.pdf) |
| XC6220A331MR-G (IC4) | [Model](https://componentsearchengine.com/part-view/XC6220A331MR-G/Torex) | [Datasheet](https://product.torexsemi.com/system/files/series/xc6220.pdf) |
