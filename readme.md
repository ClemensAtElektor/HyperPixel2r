The [HyperPixel2r](https://www.elektor.com/hyperpixel-2-1-round-hi-res-display-for-raspberry-pi) from Pimoroni is a round 2.1” IPS capacitive touchscreen with high-speed DPI interface. Like its square and rectangular HyperPixel 4 brothers, the 2r is intended for Raspberry Pi. Actually, the size is optimized for the Raspberry Pi Zero and Zero 2W but, as it has the standard 40-pin HAT connector, it can be plugged on any Raspberry Pi equipped with such a connector as long as you are careful about the mechanical side of things.

The display’s resolution is 480 by 480 pixels, but as it is round, you must, of course, subtract the corners.

*Quiz: how many pixels are lost due to the rounded corners?*

*Answer: 480 × 480 × (1 - 0.25 π) = 49,444 (almost 21.5%).*

It has 18-bit color depth (meaning 262,144 colors) and supports up to 60 frames per second (FPS). The viewing area has a 2.1” or 53.3 mm diameter and a viewing angle of 175°. Its full diameter is 72 mm with a height of 11 mm. With a Pi Zero attached with short stand-offs, the total height (or depth, whatever you prefer) is 17 mm.

As the display uses almost every pin of the HAT connector, you cannot add other extension boards. However, the display does provide an alternate I2C port for connecting things to.

To use the HyperPixel 2r on a Raspberry Pi you must install a driver: 

```
$ git clone https://github.com/pimoroni/hyperpixel2r
$ cd hyperpixel2r
$ sudo ./install.sh
$ sudo reboot
```

The drivers are for Raspberry Pi OS Buster only, but support for Bullseye is being worked on.

To use the display in your own applications, install Pimoroni's ```hyperpixel2r-python``` library:

```
$ git clone https://github.com/pimoroni/hyperpixel2r-python
$ cd hyperpixel2r-python
$ sudo ./install.sh
```

Note that touch needs a driver to make it work as a mouse on the desktop. Unfortunately, such a driver does not seem to exist yet, but you can use the library example uinput-touch.py as a deamon instead:

```
$ cd hyperpixel2r-python/examples
$ python3 uinput-touch.py
```

If the library examples look funny, upgrade pygame:

```
$ sudo python3 -m pip install pygame --upgrade
```

Note that for some reason the center of the screen buffer may not exactly be the center of the screen, it can be off in the vertical direction by several pixels. You can correct this by adding an offset, but the sign of the offset depends on the rotation of the screen. You can see in my code how I handled that. 

In order to print text on the screen with pygame you may have to install ```libsdl2-ttf```:

```
$ sudo apt-get install libsdl2-ttf-2.0-0
```

In your program you must also call ```pygame.init``` and load a font before trying to print anything to the screen.

For the YouTube subscriber counter part, you'll need also ```httplib2```:

```
$ sudo pip3 install httplib2
```

To rotate the display 180 degrees, edit ```/boot/config.txt```:

```
$ sudo nano /boot/config.txt
```

Add the line:

```
display_lcd_rotate=2
```

Save and exit (Ctrl-X, Y, Enter) and reboot:

```
$ sudo reboot
```

To find the HyperPixel's alternate I2C port use:

```
$ i2cdetect -l 
```

Or look in the ```/dev``` folder for files that start with i2c:

```
$ dir /dev 
```

In my case it is ```i2c-11```. Redirecting it to ```i2c-1``` makes it work with other libraries that expect ```i2c-1```:

```
$ sudo ln -s /dev/i2c-11 /dev/i2c-1
```

[Here is a video about this HyperPixel2r project](https://youtu.be/KEdkcJYxZQg)

Run this program:

```
$ python3 clock-ytsc.py
```

