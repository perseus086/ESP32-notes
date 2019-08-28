# ESP32-notes

## ULP low-bit, high-bit explanation

I had some problems understanding how assembler commands work for ULP on ESP32

This is an example for **I_WR_REG**


In the documentation we have I_WR_REG(reg, low_bit, high_bit, val)

We have this table for the bits RTC_GPIO and GPIO


| Bit    | RTC           | GPIO     |
| ------ |:-------------:| --------:|
|bit 14  |  RTC_GPIO 0   | GPIO 36  |
|bit 15  |  RTC_GPIO 1   | GPIO 37  |
|bit 16  |  RTC_GPIO 2   | GPIO 38  |
|bit 17  |  RTC_GPIO 3   | GPIO 39  |
|bit 18  |  RTC_GPIO 4   | GPIO 34  |
|bit 19  |  RTC_GPIO 5   | GPIO 35  |
|bit 20  |  RTC_GPIO 6   | GPIO 25  |
|bit 21  |  RTC_GPIO 7   | GPIO 26  |
|bit 22  |  RTC_GPIO 8   | GPIO 33  |
|bit 23  |  RTC_GPIO 9   | GPIO 32  |
|bit 24  |  RTC_GPIO 10  | GPIO  4  |
|bit 25  |  RTC_GPIO 11  | GPIO  0  |
|bit 26  |  RTC_GPIO 12  | GPIO  2  |
|bit 27  |  RTC_GPIO 13  | GPIO 15  |
|bit 28  |  RTC_GPIO 14  | GPIO 13  |
|bit 29  |  RTC_GPIO 15  | GPIO 12  |
|bit 30  |  RTC_GPIO 16  | GPIO 14  |
|bit 31  |  RTC_GPIO 17  | GPIO 27  |


So suppose you want to turn on a led GPIO 02 (that is the led included on the board)

I_WR_REG(RTC_GPIO_OUT_REG, 26, 26, 1)

That will do it

Now you will find some examples with:

I_WR_REG(RTC_GPIO_OUT_REG, 26, 27, 1)

### what it means is:

1 in binary is 01


bit 26 is GPIO02 ----> 1

bit 27 is GPIO15  ----> 0


GPIO02 will have 1

GPIO15 will have 0

I_WR_REG(RTC_GPIO_OUT_REG, 26, 27, 2)

GPIO02 will have **0**, GPIO15 will have **1**

I_WR_REG(RTC_GPIO_OUT_REG, 26, 27, 3)

GPIO02 will have **1**, GPIO15 will have **1**

### Another example:

I_WR_REG(RTC_GPIO_OUT_REG, 22, 26, 21)

21 in binary is 10111

bit 22 is GPIO33  -->  1

bit 23 is GPIO32  -->  0

bit 24 is GPIO04  -->  1

bit 25 is GPIO00  -->  1

bit 26 is GPIO02  -->  1


Now what is the problem.


We need to check if the GPIO is input/output or it will not work. Example RTC_GPIO0 (GPIO36) is **INPUT ONLY**

https://www.espressif.com/sites/default/files/1a-esp32_pin_list_en-v0.1.pdf
