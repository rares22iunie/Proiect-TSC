# OpenBook eBook Reader
Acest proiect reprezintă un cititor de eBook-uri portabil, cu consum redus de energie, afișaj E-Ink și conectivitate modernă, proiectat în jurul microcontrollerului ESP32-C6-WROOM-1-N8.

# Arhitectură hardware
- Microcontroller – ESP32-C6-WROOM-1-N8
- Procesor RISC-V

- Conectivitate: Wi-Fi 6 și Bluetooth 5

- Suport criptografie hardware

- Flash extern NOR de 64MB (W25Q512JVEIQ), conectat pe magistrală SPI dedicată

Interfețe: SPI, I2C, GPIO

## Sistem de alimentare
- Conector USB-C (PMF0501) – intrare standard 5V

- Protecție ESD pentru liniile USB (USBLC6-2SC6Y)

- Circuit de încărcare Li-Po (MCP73831), max. 500 mA

- Baterie Li-Po 3.7V

- Regulator LDO 3.3V (MCP1700-3302E) pentru alimentarea circuitelor

## Afișaj E-Ink
- Conector compatibil cu ecrane E-Paper de 1.54” (200x200 px)

- Circuit dedicat de generare a tensiunii (tranzistor, inductor, diodă)

- Jumper selector pentru tipul de ecran (SJ1)

- Interfață SPI + pini GPIO pentru control, conectat la ESP32-C6

## Memorie externă
- Card SD (SPI) pentru stocarea fișierelor eBook

- Memorie NOR Flash de 64MB (W25Q512JVEIQ) pentru resurse firmware (fonturi, imagini)

## Senzori și periferice
- BME688 – senzor de temperatură, umiditate, presiune, compuși volatili (I2C)

- DS3231SN – modul RTC de precizie (I2C)

- MAX17048G-T10 – monitorizare baterie (fuel gauge, I2C)

- Butoane pentru reset și boot (GPIO)

- Conector Qwiic / Stemma QT pentru extensii rapide I2C
# Block Diagram
<img width="703" alt="image" src="https://github.com/user-attachments/assets/c2d9aff6-9086-4188-b930-5818d4a67fb5" />


# Design PCB și carcasă
- Placă PCB organizată în blocuri funcționale: alimentare, MCU, afișaj, senzori

- Trasee optimizate și plan de masă comun

- Protecții ESD incluse

- Conectori externi ușor accesibili (USB, butoane, display)

- Randări 3D disponibile

- Carcasă 3D compactă și ergonomică, gata de printare/turnare

# Conexiuni ESP32-C6
| Componenta     | Interfata  | Pini ESP32-C6                      | Observatii                    |
|----------------|------------|------------------------------------|-------------------------------|
| SD Card        | SPI        | GPIO10, GPIO11, GPIO12, GPIO13     | Bus SPI pentru card SD        |
| NOR Flash      | SPI        | GPIO6, GPIO7, GPIO8, GPIO9         | Bus SPI separat               |
| E-Ink Display  | SPI + GPIO | GPIO18, GPIO19, GPIO20, GPIO21     | Control display si date       |
| BME688         | I2C        | GPIO1 (SDA), GPIO2 (SCL)           | Bus comun I2C                 |
| DS3231         | I2C        | GPIO1 (SDA), GPIO2 (SCL)           | Comun cu ceilalti senzori     |
| MAX17048       | I2C        | GPIO1 (SDA), GPIO2 (SCL)           | Comun cu ceilalti senzori     |
| Butoane        | GPIO       | GPIO0 (RST), GPIO3 (BOOT)          | Control de sistem             |
| Test Pads      | GPIO/UART  | GPIO17, GPIO22                     | Debug                         |
| Stemma/Qwiic   | I2C        | GPIO1 (SDA), GPIO2 (SCL)           | Extensii rapide               |


# Estimare consum de energie
| Modul                  | Consum mediu        |
|------------------------|---------------------|
| ESP32-C6 activ         | ~80-100 mA          |
| ESP32-C6 deep sleep    | ~10 uA              |
| E-Ink refresh          | ~25 mA (temporar)   |
| SD Card (idle/acces)   | ~2 mA / ~50 mA      |
| RTC + BME688 standby   | ~3-5 uA             |
| MAX17048               | ~3 uA               |

**Durata estimata cu baterie 500mAh:**

- Utilizare activă: 15–20 ore

- Standby (deep sleep): până la câteva săptămâni

# Bill of Materials

| Cantitate | Componentă                         | Valoare | Descriere                                 | Carcasă                         | Reper        | Link Cumpărare                                                                 | Datasheet                                                                          |
|-----------|------------------------------------|---------|-------------------------------------------|---------------------------------|--------------|----------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1         | ADAFRUIT_LEDCHIP-LED0603           | -       | LED                                       | ADAFRUIT_CHIP-LED0603           | CHG_LED      | [link](https://www.mouser.com/ProductDetail/Adafruit/CHIP-LED0603)               | [datasheet](https://www.mouser.com/datasheet/2/737/CHIP-LED0603-1877199.pdf)       |
| 1         | SJ                                 | -       | SMD solder JUMPER                         | SJ                              | SJ1          | [link](https://www.mouser.com/ProductDetail/TE-Connectivity/1-2176091-2)         | [datasheet](https://www.mouser.com/datasheet/2/418/1-2176091-2-1817091.pdf)        |
| 1         | ESP32_WROVER_EAGLE-LTSPICE_RR0402  | 0.47Ω   | Rezistor                                  | ESP32_WROVER_EAGLE-LTSPICE_R0402| R3           | [link](https://www.mouser.com/ProductDetail/Vishay/CRCW04020047ZRT7)             | [datasheet](https://www.mouser.com/datasheet/2/427/crcwce-1764191.pdf)             |
| 1         | ESP32_WROVER_EAGLE-LTSPICE_RR0402  | 100KΩ   | Rezistor                                  | ESP32_WROVER_EAGLE-LTSPICE_R0402| R1_PWRUSB    | [link](https://www.mouser.com/ProductDetail/Vishay/CRCW0402100KFKED)             | [datasheet](https://www.mouser.com/datasheet/2/427/crcwce-1764191.pdf)             |
| 8         | EAGLE-LTSPICE_CC0402               | 100nF   | Condensator                               | EAGLE-LTSPICE_C0402             | C1, C2, C4_USB, C6, C8, C9, C10, C_DELAY | [link](https://www.mouser.com/ProductDetail/Murata/GRM155R71C104KA88D)            | [datasheet](https://www.mouser.com/datasheet/2/281/GRM155R71C104KA88D-1767201.pdf) |
| 1         | RCL_CPOL-EUCT3528                  | 100µF   | Condensator polarizat                     | RCL_CT3528                      | C3           | [link](https://www.mouser.com/ProductDetail/Panasonic/EEE-FK1V101P)              | [datasheet](https://www.mouser.com/datasheet/2/315/EEE-FK1V101P-1526881.pdf)       |
| 10        | ESP32_WROVER_EAGLE-LTSPICE_RR0402  | 10KΩ    | Rezistor                                  | ESP32_WROVER_EAGLE-LTSPICE_R0402| R1, R1-PINH, R1_PINH1, R2-PINH, R2_PINH1, R4, R_BOOT, R_CHANGE, R_CL1, R_RESET | [link](https://www.mouser.com/ProductDetail/Vishay/CRCW040210K0FKED)              | [datasheet](https://www.mouser.com/datasheet/2/427/crcwce-1764191.pdf)             |
| 6         | ESP32_WROVER_EAGLE-LTSPICE_RR0402  | 10kΩ    | Rezistor                                  | ESP32_WROVER_EAGLE-LTSPICE_R0402| R5, R6, R7, R8, R9, R10 | [link](https://www.mouser.com/ProductDetail/Vishay/CRCW040210K0FKED)              | [datasheet](https://www.mouser.com/datasheet/2/427/crcwce-1764191.pdf)             |
| 1         | EAGLE-LTSPICE_CC0402               | 10µF    | Condensator                               | EAGLE-LTSPICE_C0402             | C7           | [link](https://www.mouser.com/ProductDetail/Murata/GRM155R61A106ME44D)           | [datasheet](https://www.mouser.com/datasheet/2/281/GRM155R61A106ME44D-1767201.pdf) |
| 1         | 112A-TAAR-R03_ATTEND               | -       | Micro SD Card Socket, Push-Push Type      | 112ATAARR03ATTEND               | J4           | [link](https://www.mouser.com/ProductDetail/Attend/112A-TAAR-R03)                | [datasheet](https://www.mouser.com/datasheet/2/828/112A-TAAR-R03-1222862.pdf)      |
| 1         | ESP32_WROVER_EAGLE-LTSPICE_RR0402  | 15Ω     | Rezistor                                  | ESP32_WROVER_EAGLE-LTSPICE_R0402| R_CAPACITOR  | [link](https://www.mouser.com/ProductDetail/Vishay/CRCW040215R0FKED)             | [datasheet](https://www.mouser.com/datasheet/2/427/crcwce-1764191.pdf)             |
| 1         | EAGLE-LTSPICE_CC0402               | 1µF     | Condensator                               | EAGLE-LTSPICE_C0402             | C5           | [link](https://www.mouser.com/ProductDetail/Murata/GRM155R61A105KE15D)           | [datasheet](https://www.mouser.com/datasheet/2/281/GRM155R61A105KE15D-1767201.pdf) |
| 10        | EAGLE-LTSPICE_CC0402               | 1µF/50V | Condensator ceramic multi-strat           | 0402                           | EPD_C1, EPD_C2, EPD_C5, EPD_C6, EPD_C7, EPD_C8, EPD_C9, EPD_C10, EPD_C11, EPD_C12 | [link](https://www.mouser.com/ProductDetail/Murata-Electronics/GRM155R61A105KE15D) | [datasheet](https://www.mouser.com/datasheet/2/281/1/GRM155R61A105KE15_01A-1983730.pdf) |

# Aplicații posibile
- eBook Reader offline

- Afișaj de birou cu date locale (calendar, vreme, informații personale)

- Platformă portabilă pentru aplicații IoT low-power

# Resurse
- Documentație oficială ESP32-C6: https://www.espressif.com/en/products/modules/esp32-c6
