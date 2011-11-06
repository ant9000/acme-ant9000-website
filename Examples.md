### A look at the examples ###

FoxNode library comes with some examples, written to show how easy it is to develop on the FoxBoard using NodeJS.

Here is the complete list:

*  [server side test for acme.gpio](#test_gpio.js)
*  [server side test for acme.daisy](#test_daisy.js)
*  [web app sample for GPIO, using meryl](#meryl_gpio)
*  [web app sample for Daisy, using express](#express_daisy)


<a name="test_gpio.js">A server side test for acme.gpio</a>
-----------------------------------------------------------

For this example we make use of two GPIO pins, namely J7.3 as input and J7.4 as output. Wire a push button between J7.3 and J7.40, and a led in series with a 1kOhm resistance between J7.4 and J7.1, then execute the program with

```bash
node test_gpio.js
```

and leave it running. If you press the button, the program will log the state change on the console. Moreover, the app reacts to signals USR1 and USR2: if you open another connection to the FoxBoard, the command

```bash
killall -USR1 test_gpio.js
```

will produce a dump of the current state and count for J7.3 and J7.4, while

```bash
killall -USR2 test_gpio.js
```

will toggle the state of J7.4 - you will see the led switching on and off, and a console log will also inform you of the change.


<a name="test_daisy.js">A server side test for acme.daisy</a>
-------------------------------------------------------------

For trying this example you need a Daisy1 (the Daisy connector module), a Daisy5 (8 push buttons) and a Daisy11 (8 leds). Plug the Daisy1 onto your FoxBoard, then connect the Daisy5 to D5 and the Daisy11 to D2. You can then launch the program as

```bash
node test_daisy.js
```

and leave it running. If you press any button on the Daisy5, the program will log the button state change on the console. Moreover, the app reacts to signals USR1 and USR2: if you open another connection to the FoxBoard, the command

```bash
killall -USR1 test_daisy.js
```

will produce a dump of the current state and count for every button and led of the two Daisy boards, while

```bash
killall -USR2 test_daisy.js
```

will toggle the state of L3 on Daisy11 - you will see the led switching on and off, and a console log will also inform you of the change.

<a name="meryl_gpio">Web app sample for GPIO, using meryl</a>
------------------------------------------------------------

This sample uses the same setup as the server side GPIO test, i.e. a push button connected to J7.3 and a led wired to J7.4 - but this time the interaction with the users will not be on the FoxBoard console, but on a web page.

On first usage, type

```bash
npm bundle
```

in the example directory to download and install the needed dependencies.

The example is executed as

```bash
./app.js
```

Wait a few seconds for the *socket.io started* message to appear on console, then point your browser to port 8080 of the Fox and enjoy.

Push the button: the green led on the page will light up instantly. Click on the red leds on the page, and the real led will react at once - retroacting a change on the page, too.

For more fun, point multiple browser windows to the same page and watch them as they synchronize in real time.

<a name="express_daisy">Web app sample for Daisy, using express</a>
-------------------------------------------------------------------

Before starting the example, attach a Daisy5 to connector D5 on your Daisy1 and a Daisy11 to connector D2.

On first usage, type

```bash
npm bundle
```

in the example directory to download and install the needed dependencies.

The example is executed as

```bash
./app.js
```

Wait a few seconds for the *socket.io started* message to appear on console, then point your browser to port 3000 of the Fox and enjoy.

Push any button on your Daisy5: the corresponding green led on the page will light up instantly. Click on the red leds on the page, and the Daisy11 leds will react immediately - retroacting a change on the page, too.

For more fun, point multiple browser windows to the same page and watch them as they synchronize in real time.
