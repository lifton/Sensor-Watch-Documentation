The Sensor Watch
================

The Sensor Watch is a board replacement for the classic Casio F-91W wristwatch. It is powered by a Microchip SAM L22 microcontroller with built-in segment LCD controller. You can write your own programs for the watch using the provided watch library, program the watch over USB using the built-in UF2 bootloader, and then install the board in your existing watch case to run your own software on your wrist.

![Photo: a close-up of the Sensor Watch circuit board, and a wristwatch displaying the word “Hello”](images/sensor-watch.jpg)

Features:
* ARM Cortex M0+ microcontroller
* 32KHz crystal for real-time clock with alarm function
* Ten digit segment LCD, plus five indicator segments
* Three interrupt capable buttons
* Red / green PWM’able LED backlight
* Optional piezo buzzer (requires some light soldering)
* On-board USB Micro B connector
* Reset button with double-tap UF2 bootloader
* Nine-pin flex PCB connector

![Rendering: an L-shaped flex PCB labeled “Sensor Watch Environment”](images/sensor-board.png)

You may have noticed that there are no sensors on this board. That is by design: rather than pick sensors for you, the goal is to add a tiny flexible PCB with the sensors YOU want, and interface them over the nine-pin connector. The connector provides the following options for power and connectivity:

* 3V power (nominal voltage from a CR2016 coin cell, can drop to ~2.7V)
* An I²C interface with built-in pull-up resistors
* Five general purpose IO pins, which can be configured as:
    * Five analog inputs
    * Five interrupt-capable digital inputs, with internal pull-up or pull-down resistors
    * Five digital outputs
    * SPI controller (with one spare analog / GPIO pin leftover)
    * One UART TX/RX pair (with three GPIO leftover)
    * Up to four PWM pins on two independent TC instances
    * Two external wake inputs that can wake from the ultra-low-power BACKUP mode

| **Pin** | **Digital** | **Interrupt**   | **Analog**    | **I2C**             | **SPI**              | **UART**                 | **PWM**  | **Ext. Wake** |
| :-----: | :---------: | :-------------: | :-----------: | :-----------------: | :------------------: | :----------------------: | :------: | :-----------: |
| **A0**  | PB04        | EIC/EXTINT\[4\] | ADC/AIN\[12\] | —                   | —                    | —                        | —        | —             |
| **SCL** | —           | —               | —             | SCL<br>SERCOM1\[1\] | —                    | —                        | —        | —             |
| **SDA** | —           | —               | —             | SDA<br>SERCOM1\[0\] | —                    | —                        | —        | —             |
| **A1**  | PB01        | EIC/EXTINT\[1\] | ADC/AIN\[9\]  | —                   | SCK<br>SERCOM3\[3\]  | RX<br>SERCOM3\[3\]       | TC3\[1\] | —             |
| **A2**  | PB02        | EIC/EXTINT\[2\] | ADC/AIN\[10\] | —                   | MOSI<br>SERCOM3\[0\] | TX or RX<br>SERCOM3\[0\] | TC2\[0\] | RTC/IN\[1\]   |
| **A3**  | PB03        | EIC/EXTINT\[3\] | ADC/AIN\[11\] | —                   | CS<br>SERCOM3\[1\]   | RX<br>SERCOM3\[1\]       | TC2\[1\] | —             |
| **A4**  | PB00        | EIC/EXTINT\[0\] | ADC/AIN\[8\]  | —                   | MISO<br>SERCOM3\[2\] | TX or RX<br>SERCOM3\[2\] | TC3\[0\] | RTC/IN\[0\]   |

These tiny “sensor boards” have a set outline, and the available area for your electronics is quite small (5.7 × 5.7 × 1 mm). Still, this is plenty of room for an environmental sensor, MEMS accelerometer or magnetometer and a couple of decoupling capacitors. Note that you will likely be limited to QFN and LGA type parts; SOICs are too large, and even SSOP packages are generally too thick. You can find reference designs for several sensor boards in [the PCB/Sensor Boards directory](https://github.com/joeycastillo/Sensor-Watch/tree/main/PCB/Sensor%20Boards) in the Sensor Watch repository.
