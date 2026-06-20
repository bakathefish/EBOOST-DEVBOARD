<img width="1410" height="2000" alt="Copy of ASTRO PET (2)" src="https://github.com/user-attachments/assets/a5b7da23-bc1c-4a3c-9f6f-94539218292f" />


# EBOOST-DEVBOARD

An esp32-s3 devboard that has a raw epaper module thing so you don't need to spend time figuring out why your epaper screen isn't working. Normal epaper screens need a full voltage booster panel and a custom circuit. Epaper screen modules where i live are very hard to find while the raw screens are much easier, since i wanted to make cool projects with epaper screens i decided to make my own module to make it very easy.

## WHAT IT HAS

| | |
|---|---|
| MCU | ESP32-S3-WROOM-1-N16R8 |
| Memory | 16 MB flash, 8 MB octal psram |
| Wireless | Wi-Fi 2.4 GHz, Bluetooth 5 |
| USB | Native USB-C |
| Extra | Native E-Paper support |

## how the epaper works

a raw epaper screen is just glass and a flexible cable, there is no power circuitry on it, nothing. you may think you can just plug it into any gpios and it would work, but that's not the case. epaper screens need a high positive rail of around +22v and a high negative rail of around -20v to make their images.

after the image has been made the power can be removed without any consequences, but to generate the image you need the custom circuitry.

## THE DEVBOARD

SCHEMATIC

<img width="1252" height="757" alt="schematic" src="https://github.com/user-attachments/assets/88dec064-02ce-4e1e-a0d9-89abc34576f7" />


THE PCB



<img width="826" height="768" alt="image (10)" src="https://github.com/user-attachments/assets/1d77783a-59f5-4dee-8483-5cedc89d382c" />



## the screen pins

the epaper screen talks to the esp32 over spi. this is what connects where:

| signal | gpio |
|---|---|
| CS | IO10 |
| MOSI | IO11 |
| SCK | IO12 |
| DC | IO4 |
| RST | IO5 |
| BUSY | IO6 |
| booster enable | IO7 |

booster enable is the important one, it turns the high voltage circuit on. flip it high right before a refresh and back low after, so the board isn't making +22v for no reason.

usb sits on IO19 (D-) and IO20 (D+), the reset button is on EN, and the boot button is on IO0.

## exposed gpios

the esp32-s3 has way more pins than this board needs, so every gpio that's not already in use is broken out on 2.54mm pads, with 3v3, 5v and gnd right next to them. solder headers or just wires and you can hang whatever you want off it.

what's already taken: the 7 screen pins above, usb (IO19, IO20), and the 3 pins the octal psram uses inside the module (IO35, IO36, IO37). everything else is free.

that leaves 21 free gpios on the pads:

1, 2, 3, 8, 9, 13, 14, 15, 16, 17, 18, 21, 38, 39, 40, 41, 42, 45, 46, 47, 48

(IO3, IO45 and IO46 are strapping pins, so don't hold them high while the board is booting.)

## assembly

- get your pcbs made first. order the gerbers from jlcpcb and grab the parts off the BOM below.
- every part is labelled on the board's silkscreen, so just place each one where its marking is.
- solder the small parts first, the resistors and caps, then move up to the bigger stuff like the module and the connectors. doing the tiny ones first makes it way easier to reach everything.
- a hotplate or hot air helps a lot with the module's centre pad and the small fpc connector, but you can hand solder it too.

## using it

once it's together you flash it over usb-c and use it like any normal esp32 board.

- it has a reset button and a boot button, same as any esp dev board. if it doesn't jump into flashing mode on its own, hold boot, tap reset, let go, then flash.
- plug a raw epaper panel into the fpc connector and draw to it. remember to pull the booster enable pin high before a refresh.
- the free gpios on the pads are there to add more to your build, sensors, buttons, a battery, whatever you need.

## BOM

| Qty | Description | Part No. | MPN | Price/Qty (USD) | Total (USD) |
|---|---|---|---|---|---|
| 1 | 22uF Cap | [C86295](https://www.lcsc.com/product-detail/C86295.html) | CL10A226MP8NUNE | 0.038 | 0.038 |
| 6 | 100nF Cap | [C1591](https://www.lcsc.com/product-detail/C1591.html) | CL10B104KB8NNNC | 0.0115 | 0.069 |
| 2 | 22Ω RES | [C2907129](https://www.lcsc.com/product-detail/C2907129.html) | FRC0603J220 TS | 0.0018 | 0.0036 |
| 1 | ESP 32 | [C2913202](https://www.lcsc.com/product-detail/C2913202.html) | ESP32-S3-WROOM-1-N16R8 | 5.4159 | 5.4159 |
| 2 | 5.1Ω RES | [C2907044](https://www.lcsc.com/product-detail/C2907044.html) | FRC0603F5101TS | 0.0022 | 0.0044 |
| 1 | TYPE C | [C2765186](https://www.lcsc.com/product-detail/C2765186.html) | TYPE-C 16PIN 2MD(073) | 0.0707 | 0.0707 |
| 3 | 10Ω RES | [C2930027](https://www.lcsc.com/product-detail/C2930027.html) | FRC0603J103 TS | 0.0013 | 0.0039 |
| 2 | Swtich | [C52092028](https://www.lcsc.com/product-detail/C52092028.html) | SH-1808SN-250gf | 0.0287 | 0.0574 |
| 1 | EP-Connector | [C519265](https://www.lcsc.com/product-detail/C519265.html) | X05A26H24G | 0.4197 | 0.4197 |
| 9 | 1uF Cap | [C913562](https://www.lcsc.com/product-detail/C913562.html) | GRM188B31H105KAALD | 0.0619 | 0.5571 |
| 3 | 10uF Cap | [C96446](https://www.lcsc.com/product-detail/C96446.html) | CL10A106MA8NRNC | 0.0533 | 0.1599 |
| 1 | LDO | [C51118](https://www.lcsc.com/product-detail/C51118.html) | AP2112K-3.3TRG1 | 0.1579 | 0.1579 |
| 1 | COIL | [C72424](https://www.lcsc.com/product-detail/C72424.html) | SM3521-680MT | 0.0402 | 0.0402 |
| 1 | 4.7uF Cap | [C19666](https://www.lcsc.com/product-detail/C19666.html) | CL10A475KO8NNNC | 0.0218 | 0.0218 |
| 1 | 470mΩ Cap | [C186564](https://www.lcsc.com/product-detail/C186564.html) | SR731JTTDR470F | 0.1195 | 0.1195 |
| 1 | AOS | [C485690](https://www.lcsc.com/product-detail/C485690.html) | AO7400 | 0.0695 | 0.0695 |
| 3 | JSCJ MBR0530 | [C77336](https://www.lcsc.com/product-detail/C77336.html) | MBR0530 | 0.0368 | 0.1104 |

Total: about 7 dollars
