# Seven Calendar Cafe

![screenshot](https://cdn.glitch.com/b8a38280-42c0-4d93-921c-b87f4ba67e1a%2Fseven-calendar-cafe.jpg?v=1609057836948 "A screenshot of the HTML produced by this project. Shown is a calendar for December 2020, weeks starting on Sunday and the 26th highlighted as the current date.")

This is a static HTML project which uses JavaScript to generate a calendar for the current month (by default) according to the clock on the browser displaying it.

The generated HTML is intended for display on an eInk display, via a screen capture by running a browser on the command line, or even a screenshot service.

The CSS on this project is optimized for display on [Pimoroni's seven-color eInk display for the Raspberry Pi](https://shop.pimoroni.com/products/inky-impression).

## Motivation

Leverage a browser's HTML and CSS engine to lay out the calendar for us. 

The project was inspired by a [three-line layout using CSS Grid](https://calendartricks.com/a-calendar-in-three-lines-of-css/) to lay out the calendar. 

Adding a custom background, different typography, or even date-sensitive styling is a straightforward extension of the project.

## Usage

This project accepts the optional query string parameter `month` which can take on the value `previous` and `next` to display the calendar for the previous and next months.

The current date is highlighted when the calendar is displayed.

## Using with eInk Display

I am able to run this on a stock RaspberryPi Zero W. Install the fully RaspberryPi OS with desktop. If you use the RaspberryPi imager you can specify a user to create as well as pre-fill the WiFi credentials. Be sure to enable ssh login. 

Once the eInk display is attached to the Pi and the necessary drivers and libraries are installed, you can fetch a screenshot with the current calendar using:

``` bash
chromium-browser --headless --screenshot="~/screenshot.png" --window-size=600,448 "https://7-calendar-cafe.glitch.me"
```
or

``` bash
firefox --headless --screenshot --window-size=600,448 'https://7-calendar-cafe.glitch.me/'
```
I have not been able to run Firefox as a headless browser from the command line on Pi Zero W.

You need to be specific about the `--window-size` parameter for the dimensions of your eInk display's screen.

You'll need a script (in this case Python) which works with the Pimoroni eInk display's libraries:

``` python
#!/usr/bin/env python3

import sys

from PIL import Image
from inky.inky_uc8159 import Inky

inky = Inky()
saturation = 0.5

image = Image.open("/home/pi/screenshot.png")

inky.set_image(image, saturation=saturation)
inky.show()
```

And a bash script can run the process daily once you set up a `crontab` entry for it.

``` bash
cd $HOME
chromium-browser --headless --screenshot="~/screenshot.png" --window-size=600,448 "https://7-calendar-cafe.glitch.me" 
./calendar.py
```

Since the idea behind eInk is to only update when you need to, using a whole Raspberry Pi (even a Pi Zero W) feels like one's over specified the hardware for the project. 

The 7-color Pimoroni display has buttons you can program, and using them will be the next version of this project.

Show off your mods with the hashtag #SevenCalendarCafe on Twitter and the Fediverse.

The project's title is a nod to both the Pimoroni Inky Impression and [The Cocteau Twins](https://www.discogs.com/Cocteau-Twins-Four-Calendar-Caf%C3%A9/master/5057).
