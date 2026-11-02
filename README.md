# CO2 Buddy

Battery-powered CO2 monitor on an ESP32-C3. Reads an MH-Z19C over PWM, shows the value on a 1.54" e-paper display and an animated face on an 8×8 WS2812B matrix, then deep-sleeps.

## BOM

- ESP32-C3 SuperMini
- MH-Z19C CO2 sensor
- 8×8 WS2812B LED matrix
- 1.54" e-paper display (GDEY0154D67)
- TTP223 touch sensor
- LX-LCBST charging module
- 3000 mAh LiPo battery
- Slide switch
- M3 screws

## Wiring

| Part | ESP32-C3 pins |
| --- | --- |
| MH-Z19C, PWM output | GPIO 10 |
| E-paper GDEY0154D67 (SPI) | BUSY 0, RST 1, DC 2, CS 20, MOSI 5, SCK 7 |
| WS2812B matrix, data in | GPIO 8 |
| TTP223 output (active high) | GPIO 3 |

## Behaviour

- First power-on: 70 s sensor preheat with a fire animation. The e-paper shows the remaining time in 10 s steps in the top-left corner, using partial refresh.
- Air quality bands and faces:

  | Band | CO2 (ppm) | Face |
  | --- | --- | --- |
  | GOOD | < 600 | green, smiling |
  | OKAY | 600–999 | yellow, neutral |
  | BAD | 1000–1499 | orange, frowning |
  | TOXIC | ≥ 1500 | red, angry |

  A failed PWM read shows ERROR.
- The device wakes every 150 s, takes a reading and updates the e-paper with the ppm value and band. The e-paper keeps the image while the device sleeps.
- The LED face is only lit while awake: 3 s at low brightness if the band is unchanged, 5 s at normal brightness if it changed.
- Touching the sensor wakes the device, plays a short animation, shows the last band and takes a new reading.

## Build

PlatformIO, board definition `lolin_c3_mini`:

```sh
pio run -t upload
pio device monitor -b 115200
```

Library dependencies are listed in `platformio.ini`.

## License

MIT
