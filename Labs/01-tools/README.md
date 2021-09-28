# Lab 1: Matej Ledvina (221339)
[Link to my github](https://github.com/Ledvuk/Digital-electronics-2/)
### Blink example

1. What is the meaning of the following binary operators in C?
   * `|` OR
   * `&` AND
   * `^` XOR
   * `~` NOT
   * `<<` BIT SHIFT LEFT
   * `>>` BIT SHIFT RIGHT

2. Complete truth table with operators: `|`, `&`, `^`, `~`

| **b** | **a** |**b or a** | **b and a** | **b xor a** | **not b** |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0 | 0 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 | 1 |
| 1 | 0 | 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 |


### Morse code

1. Listing of C code with syntax highlighting which repeats one "dot" and one "comma" (BTW, in Morse code it is letter `A`) on a LED:

```c

#define LED_GREEN   PB5 
#define SHORT_DELAY 50 
#define LONG_DELAY 150

int main(void)
{
    // Set pin as output in Data Direction Register
    // DDRB = DDRB or 0010 0000
    DDRB = DDRB | (1<<LED_GREEN);

    // Set pin LOW in Data Register (LED off)
    // PORTB = PORTB and 1101 1111
    PORTB = PORTB & ~(1<<LED_GREEN);

    // Infinite loop
    while (1)
    {
        //DOT
	_delay_ms(SHORT_DELAY);
        PORTB = PORTB ^ (1<<LED_GREEN);
	_delay_ms(SHORT_DELAY);
        PORTB = PORTB ^ (1<<LED_GREEN);

	//DASH
	_delay_ms(SHORT_DELAY);
        PORTB = PORTB ^ (1<<LED_GREEN);
	_delay_ms(LONG_DELAY);
        PORTB = PORTB ^ (1<<LED_GREEN);
    }

    // Will never reach this
    return 0;
}
```


2. Scheme of Morse code application, i.e. connection of AVR device, LED, resistor, and supply voltage. The image can be drawn on a computer or by hand. Always name all components and their values!

   ![your figure]()