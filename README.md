# RGBLED

Library for controlling RGB LED for ARDUINO/Genuino. Written in C++.

Tags: Arduino RGB LED library color brightness

This library is created to simplify the steering of a colorful LED. The light emitted from the LED is set through the three color components, i.e. red, green and blue. The brightnesses of the three separate color channels of the LED are changed using PWM. The library itself uses `analogWrite(...)` to change the PWM pulse width. 

###1. LICENSE

[Creative Commons Attribution-ShareAlike 3.0 License](http://creativecommons.org/licenses/by-sa/3.0/)

###2. USAGE

Copy the RGBLED folder into your library folder and include it in your sketch. If you don't see the library immediately, then restart the arduino IDE.

Create a RGBLED object. In order to create a RGBLED object, you must specify the pin numbers of the three separate color channels of the RGB LED as input parameters to the constructor. The first parameter is the pin number of the red color. The second parameter is the pin number of the green color. The third parameter is the pin number of the blue color.

You can have more than one RGBLED objects in your sketch, but do not to use the same pin number for multiple RGBLED objects.

Initially, the color is set to black, that is no light is emitted.

###3. EXAMPLE USAGE

* Import the lib into your sketch

```cpp
#import <RGBLED.h>
```

* Create an RGBLED object. As parameters provide the colors pin numbers.

```cpp
RGBLED myLed = RGBLED(pinRed, pinGreen, pinBlue);
```

* Set the color you wish

```cpp
myLed.setRed(0.1);
myLed.setGreen(0.5);
myLed.setBlue(1.0);

// OR

myLed.setRGB(0.1,0.5,1.0);
```

* You can brighten or darken the LED

```cpp
myLed.brighten(0.25);
myLed.darken(1.0);
```

* You can override the color if you need to change the color for a short time.

```cpp
// set color to dim green
myLed.setRGB(0.0,0.5,0.0);

// Override the color with full red
myLed.overrideRed();
...

// Back to previously set color. (dim green in our case)
myLed.stopOverride();
...
```

###4. THE FULL API

**1. Constructor**

```cpp
RGBLED(int redPin, int greenPin, int bluePin);
```

The constructor changes the supplied pins modes to "OUTPUT". The initial color is black, that is no light. If you assign a pin to the RGBLED object, don't use the pin for other purposes.

**2. Setting a color**

```cpp
setRed(float redValue);
setGreen(float greenValue);
setBlue(float blueValue);
setRGB(float redValue, float greenValue, float blueValue);
```

The input parameter should be in the range between 0.0 and 1.0. Setting the value of one color does not change the value of the other colors. The LED output color is the mixture of the component colors, red green and blue. Internally `analogWrite(...)` is used.

**3. Get previously set color**

```cpp
float getRed();
float getGreen();
float getBlue();
```

The values returned from this functions are the color values from the inner buffer, not necessarily the real values of the PWM. The value of PWM can be different from the buffered colors values if "autoUpdate" is disabled and the user did'nt call `update()` after setting the color value.

**4. Brightening and darkening the light**

```cpp
brighten(float amount);
brightenRed(float amount);
brightenGreen(float amount);
brightenBlue(float amount);
darken(float amount);
darkenRed(float amount);
darkenGreen(float amount);
darkenBlue(float amount);
```

The input parameter should be in the range of 0.0 to 1.0. The resulting color value is calculated to be between 0.0 and 1.0, so you can brighten the value even if it has the full brightness already.

**5. Overriding a color**

```cpp
overrideRed();
overrideGreen();
overrideBlue();
overrideWhite();
overrideBlack();
overrideRGB(float redValue, float greenValue, float blueValue);
stopOverride();
```

The overriding of the colour does not influence the color value stored in the buffer. So after calling `stopOverride()` You always get the originally set color.

**6. Disable automatic color update**

The default behaviour is that the color changes immediately after setting the color with e.g. `setRGB(...)` or `overrideBlack()`. However, sometimes you would like to buffer the color changes, and send the color value to the PWM later on. For that purpose you have the function:

```cpp
disableAutoUpdate();
```

If you call it, then after setting the color you have to manualy do updates, like so:

```cpp
// Lets say the color was black.

setRed(0.75);
// still black
setGreen(0.0);
// still black
setBlue(0.0);
// still black
update();
// the colour is now 3/4 red

overrideWhite();
// the colour did not change
update();
// now the override kicks in.
```

You can go back to automatic updates with this function:

```cpp
enableAutoUpdate();
```

You can check if automatic update is on with:

```cpp
boolean isAutoUpdate();
```

## 

Created 24 August 2014 by Marcin Drzymala
