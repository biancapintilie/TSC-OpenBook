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


