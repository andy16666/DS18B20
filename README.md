# DS18B20 #

This library was forked to correct 2 issues. 

1) The library didn't allow for multiple sensors to be read without multiple delays for conversation. This meant it got very slow with multiple sensors. 
2) The CRC was NOT checked during the actual sensor read, leading to massive numbers of read errors. 

In correcting these issues, it now enforces the following workflow: 
1) discover sensors first, using the search function to get the full addresses. (selectNext) 

During read, you must do a doConversion() first, followed by a select(), checking for the CRC failure condition by verifying that select() returns true/1. Then call getTempC() to get the contents of the scratch pad, if select() returns in the affirmative. 

The result is: 
- Only 1 delay per conversion for n sensors. 
- Eliminates a second readScratchpad() call which shouldn't exist, and which had no CRC check. 
- Eliminates the huge number of errors you'll see without a checked CRC. 


# Original Documentation (Obsolete) # 


Arduino library for the Maxim Integrated DS18B20 1-Wire temperature sensor. This library is very simple and intuitive to use, and supports auto-discovering sensors with an optional high/low condition or manually addressing individual sensors.

For example, we can get the temperature from every sensor on the wire with just a few lines of code:

```
#include <DS18B20.h>

DS18B20 ds(2);

void setup() {
  Serial.begin(9600);
}

void loop() {
  while (ds.selectNext()) {
    Serial.println(ds.getTempC());
  }
}
```

See the included [examples](/examples/) for more.

## Installation ##

This library uses the OneWire library, so you will need to have this installed. Install it using the Library Manager in the Arduino IDE or download the latest release from [GitHub](https://github.com/PaulStoffregen/OneWire).

In the **OneWire.h** file set `ONEWIRE_SEARCH` to 0 since the search functionality is also implemented in this library (don't do this if you need the search functionality for other 1-Wire devices). CRC must be enabled (choose whichever algorithm you prefer). This may save some space on your Arduino.

## Wiring the DS18B20 ##
The resistor shown in all the circuit diagrams is 4.7k Ohm pullup resistor.

### External Power Mode ###

#### Single ####
![A single externally powered DS18B20](/extras/single_external.png)

#### Multiple ####
![Multiple externally powered DS18B20s](/extras/multiple_external.png)

### Parasitic Power Mode ###

#### Single ####
![A single parasite powered DS18B20](/extras/single_parasite.png)

#### Multiple ####
![Multiple parasite powered DS18B20s](/extras/multiple_parasite.png)

### Mixed Power Mode ###
![Mixed mode DS18B20s](/extras/mixed_mode.png)
