The HyperPixel 2r from Pimoroni is a round 2.1” IPS (In-Plane Switching) capacitive touchscreen with high-speed DPI interface. Like its square and rectangular HyperPixel 4 brothers, the 2r is intended for Raspberry Pi. Actually, the size is optimized for the Raspberry Pi Zero and Zero 2W but, as it has the standard 40-pin HAT connector, it can be plugged on any Raspberry Pi equipped with such a connector as long as you are careful about the mechanical side of things.

The display’s resolution is 480 by 480 pixels, but as it is round, you must, of course, subtract the corners.

[Quiz: how many pixels are lost due to the rounded corners? 
480 × 480 × (1 - 0.25 π) = 49,444 (= 12,361 pixels per corner), almost 21.5%.
I hope I got that right.]

It has 18-bit color depth (meaning 262,144 colors) and supports up to 60 frames per second (FPS). The viewing area has a 2.1” or 53.3 mm diameter and a viewing angle of 175°. Its full diameter is 72 mm with a height of 11 mm. With a Pi Zero attached with short stand-offs, the total height (or depth, whatever you prefer) is 17 mm.

As the display uses almost every pin of the HAT connector, you cannot add other extension boards. However, the display does provide an alternate I2C port for connecting things to.

To use the HyperPixel 2r on a Raspberry Pi you must install a driver: 

’’’
$ git clone https://github.com/pimoroni/hyperpixel2r
$ cd hyperpixel2r
$ sudo ./install.sh
$ sudo reboot
’’’

The drivers are for Raspberry Pi OS Buster only, but support for Bullseye is being worked on.

To use the display in your own applications, install this Python3 library:

’’’
$ git clone https://github.com/pimoroni/hyperpixel2r-python
$ cd hyperpixel2r-python
$ sudo ./install.sh
’’’

If the library examples look funny, upgrade pygame:

’’’
$ sudo python3 -m pip install pygame --upgrade
’’’

In order to print text on the screen with pygame you might have to install libsdl2-ttf:

’’’
$ sudo apt-get install libsdl2-ttf-2.0-0
’’’

For the YouTube subscriber counter part, you'll need also httplib2:

’’’
$ sudo pip3 install httplib2
’’’

To rotate the display edit /boot/config.txt:

’’’
$ sudo nano /boot/config.txt
’’’

Add the line:

’’’
display_lcd_rotate=2
’’’

Save and exit (Ctrl-X, 'Y', Enter) and reboot:

’’’
$ sudo reboot
’’’

To find the HyperPixel's alternate I2C port use:

’’’
$ i2cdetect -l 
’’’

Or look in the /dev folder for files that start with i2c:

’’’
$ dir /dev 
’’’
