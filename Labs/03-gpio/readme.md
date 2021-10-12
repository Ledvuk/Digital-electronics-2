# Lab 3: Matej Ledvina (221339)

[Link to my github](https://github.com/Ledvuk/Digital-electronics-2/)
### Data types in C

1. Complete table.

| **Data type** | **Number of bits** | **Range** | **Description** |
| :-: | :-: | :-: | :-- | 
| `uint8_t`  | 8 | 0, 1, ..., 255 | Unsigned 8-bit integer |
| `int8_t`   | 8 | -128, ..., 0, ..., 127 | Signed 8-bit integer |
| `uint16_t` | 16 | 0, ..., 65536 | Signed 16-bit integer |
| `int16_t`  | 16 | -32768, ..., 0, ..., 32767 | Signed 16-bit, integer |
| `float`    | 32 | -3.4e+38, ..., 3.4e+38 | Single-precision floating-point |
| `void`     | x | x | no return function parameter |


### GPIO library

1. In your words, describe the difference between the declaration and the definition of the function in C.
   * Function declaration
	Lets the interpreter know that there is a function in the program. Does not contain the function working code, must be locted at the begining before the infinite loop
   * Function definition
	Contains the whole function and it's internal code. For sake of clarity should be located at the end of the entire program after the infinite loop. If the function is defined before it's called in the program no declaration at the begining is necessary.

2. Part of the C code listing with syntax highlighting, which toggles LEDs only if push button is pressed. Otherwise, the value of the LEDs does not change. Use function from your GPIO library. Let the push button is connected to port D:

```c
int main(void)
{
    // Green LED at port B
    GPIO_config_output(&DDRB, LED_GREEN);
    GPIO_write_low(&PORTB, LED_GREEN);

    // Configure the second LED at port C
    GPIO_config_output(&DDRC, LED_EXT);
    GPIO_write_high(&PORTC, LED_EXT);


    // Configure Push button at port D and enable internal pull-up resistor
    GPIO_config_input_pullup(&DDRD, P_BUTTON);

    // Infinite loop
    while (1)
    {
        if(GPIO_read(&DDRD, P_BUTTON)==0)
        {
            GPIO_toggle(&DDRB, LED_GREEN);
            GPIO_toggle(&DDRC, LED_EXT);
            // Pause several milliseconds
            _delay_ms(BLINK_DELAY);
            
            
        }
    }
    return 0;
}
    
```


### Traffic light

1. Scheme of traffic light application with one red/yellow/green light for cars and one red/green light for pedestrians. Connect AVR device, LEDs, resistors, one push button (for pedestrians), and supply voltage. The image can be drawn on a computer or by hand. Always name all components and their values!

   ![Traffic](https://github.com/Ledvuk/Digital-electronics-2/blob/main/Labs/03-gpio/traffic.png)