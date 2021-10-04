# Digital-electronics-2 Matej Ledvina (221339)
[Link to my github](https://github.com/Ledvuk/Digital-electronics-2/)
## 02-LEDs
### Preparation:

1. Draw two basic ways to connect a LED to the output pin of the microcontroller: LED active-low, LED active-high.

![LED_hookup](https://github.com/Ledvuk/Digital-electronics-2/blob/main/Labs/02-leds/LED_hookup.png)

2. [Calculate LED resistor value](https://electronicsclub.info/leds.htm) for typical red and blue LEDs.

&nbsp;
![ohms law](Images/ohms_law.png)
&nbsp;

| **LED color** | **Supply voltage** | **LED current** | **LED voltage** | **Resistor value** |
| :-: | :-: | :-: | :-: | :-: |
| red | 5&nbsp;V | 20&nbsp;mA | 1,7V | 165Ω |
| blue | 5&nbsp;V | 20&nbsp;mA | 2,8V | 110Ω |

3. Draw the basic ways to connect a push button to the microcontroller input pin: button active-low, button active-high.

![Button_hookup](https://github.com/Ledvuk/Digital-electronics-2/blob/main/Labs/02-leds/Button_hookup.png)


<a name="part2"></a>
## Part 2: Active-low and active-high LEDs

AVR microcontroller associates pins into so-called ports, which are marked with the letters A, B, C, etc. Each of the pins is controlled separately and can function as an input (entry) or output (exit) point of the microcontroller. Control is possible exclusively by software via control registers.

There are exactly three control registers for each port: DDR, PORT and PIN, supplemented by the letter designation of the port. For port A these are registers DDRA, PORTA and PINA, for port B registers DDRB, PORTB, PINB, etc.

DDR (Data Direction Register) is used to set the input/output direction of port communication, PORT is the output data port and PIN works for reading input values from the port.

A detailed description of working with input/output ports can be found in [ATmega328P datasheet](https://www.microchip.com/wwwproducts/en/ATmega328p) in section I/O-Ports.

Use the datasheet to find out the meaning of the DDRB and PORTB control register values and their combinations. (Let PUD (Pull-up Disable) bit in MCUCR (MCU Control Register) is 0 by default.)

| **DDRB** | **Description** |
| :-: | :-- |
| 0 | Input pin |
| 1 | Output pin |

| **PORTB** | **Description** |
| :-: | :-- |
| 0 | Output low value |
| 1 | Output high value |

| **DDRB** | **PORTB** | **Direction** | **Internal pull-up resistor** | **Description** |
| :-: | :-: | :-: | :-: | :-- |
| 0 | 0 | input | no | Tri-state, high-impedance |
| 0 | 1 | input | yes | Inputp pull-up |
| 1 | 0 | output | no | output low state |
| 1 | 1 | output | yes | Output high state |

See [schematic of Arduino Uno board](../../Docs/arduino_shield.pdf) in docs folder of Digital-electronics-2 repository and find out which pins of ATmega328P can be used as input/output pins. To which pin is the LED L connected? Is it connected as active-low or active-high? Note that labels on Arduino `~3`, `~5`, etc. do not mean that the signals are inverted; the `~` symbol indicates that a PWM (Pulse-width modulation) signal can be generated on these pins.

| **Port** | **Pin** | **Input/output usage?** |
| :-: | :-: | :-- |
| A | x | Microcontroller ATmega328P does not contain port A |
| B | 0 | Yes (Arduino pin 8) |
|   | 1 | Yes (Arduino pin ~9) |
|   | 2 | Yes (Arduino pin ~10) |
|   | 3 | Yes (Arduino pin ~11) |
|   | 4 | Yes (Arduino pin 12) |
|   | 5 | Yes (Arduino pin 13) |
|   | 6 | NC |
|   | 7 | NC |
| C | 0 | Yes (Arduino pin A0) |
|   | 1 | Yes (Arduino pin A1) |
|   | 2 | Yes (Arduino pin A2) |
|   | 3 | Yes (Arduino pin A3) |
|   | 4 | Yes (Arduino pin A4 + SDA) |
|   | 5 | Yes (Arduino pin A5 + SCL) |
|   | 6 | NC |
|   | 7 | NC |
| D | 0 | Yes (Arduino pin RX<-0) |
|   | 1 | Yes (Arduino pin RX->0) |
|   | 2 | Yes (Arduino pin 2) |
|   | 3 | Yes (Arduino pin ~3) |
|   | 4 | Yes (Arduino pin 4) |
|   | 5 | Yes (Arduino pin ~5) |
|   | 6 | Yes (Arduino pin ~6) |
|   | 7 | Yes (Arduino pin 8) |

Code:
```c
	int main(void)
{
    //input output setup
    PORTB = PORTB & ~(1<<LED_INT);
    DDRB = DDRB | (1<<LED_INT);
    
    PORTC = PORTC | (1<<LED_EXT);
    DDRC = DDRC | (1<<LED_EXT);

    PORTB = PORTB ^ (1<<LED_INT);

    // Infinite loop
    while (1)
    {
            _delay_ms(SHORT_DELAY);
            PORTB = PORTB ^ (1<<LED_INT);
            PORTC = PORTC ^ (1<<LED_EXT);
    }
    return 0;
}
```

## Part 3: Push button
Use code from previous part and program an application that toggles LEDs only if push button is pressed. Otherwise, the value of the LEDs does not change. 

Configure the pin to which the push button is connected as an input and enable the internal pull-up resistor.

Code:
```c
int main(void)
{
    //input output setup
    PORTB = PORTB & ~(1<<LED_INT);
    DDRB = DDRB | (1<<LED_INT);
    
    PORTC = PORTC | (1<<LED_EXT);
    DDRC = DDRC | (1<<LED_EXT);
    
    PORTC = PORTC | (1<<LED_EXT);
    DDRC = DDRC & ~(1<<LED_EXT);
    
    
    PORTB = PORTB ^ (1<<LED_INT);

    // Infinite loop
    while (1)
    {
        if (bit_is_clear(PINC, 0 )
        {
            _delay_ms(SHORT_DELAY);
            PORTB = PORTB ^ (1<<LED_INT);
            PORTC = PORTC ^ (1<<LED_EXT);
        }
    }
    return 0;
}
```
## Experiments on your own
1. Connect at least five LEDs and a push button to the microcontroller, modify `02-leds` code, and program an application in Knight Rider style.
![knight_rider](https://github.com/Ledvuk/Digital-electronics-2/blob/main/Labs/02-leds/knight_rider.png)
