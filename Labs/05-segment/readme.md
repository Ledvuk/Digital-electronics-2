# Lab 5: Matej Ledvina

[Link to my github](https://github.com/Ledvuk/Digital-electronics-2/)

### 7-segment library

1. In your words, describe the difference between Common Cathode and Common Anode 7-segment display.
   * CC SSD all cathodes of the individual LED's are connected together to common ground terminal. Segments are activated by connecting supply voltage to the individual anodes (through a current limiting resistor).
   * CA SSD all anodes of the individual LED's are connected together to common supply voltage. Segments are activated by connecting individual cathodes to ground (through a current limiting resistor).

2. Code listing with syntax highlighting of two interrupt service routines (`TIMER1_OVF_vect`, `TIMER0_OVF_vect`) from counter application with at least two digits, ie. values from 00 to 59:

```c
/**********************************************************************
 * Function: Timer/Counter1 overflow interrupt
 * Purpose:  Increment counter value from 00 to 59.
 **********************************************************************/
uint8_t dig1 = 0;
uint8_t dig2 = 0;
ISR(TIMER1_OVF_vect){
    dig1++;
    if (dig1 >= 10){
        dig1 = 0;
        dig2++;
        if (dig2 >= 6){dig2 = 0;}
    }
}
```

```c
/**********************************************************************
 * Function: Timer/Counter0 overflow interrupt
 * Purpose:  Display tens and units of a counter at SSD.
 **********************************************************************/
ISR(TIMER0_OVF_vect)
{
    static uint8_t pos = 0;
    if (position == 0){ SEG_update_shift_regs(digit1,0);}
    else{ SEG_update_shift_regs(digit2,1);}
}
```

3. Flowchart figure for function `SEG_clk_2us()` which generates one clock period on `SEG_CLK` pin with a duration of 2&nbsp;us. The image can be drawn on a computer or by hand. Use clear descriptions of the individual steps of the algorithms.

   ![your figure]()


### Kitchen alarm

Consider a kitchen alarm with a 7-segment display, one LED and three push buttons: start, +1 minute, -1 minute. Use the +1/-1 minute buttons to increment/decrement the timer value. After pressing the Start button, the countdown starts. The countdown value is shown on the display in the form of mm.ss (minutes.seconds). At the end of the countdown, the LED will start blinking.

1. Scheme of kitchen alarm; do not forget the supply voltage. The image can be drawn on a computer or by hand. Always name all components and their values.

   ![kitchen](https://github.com/Ledvuk/Digital-electronics-2/blob/main/Labs/05-segment/kitchen.png)