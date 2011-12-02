# Node-js and FoxNode examples

FoxNode library comes with some examples, written to show how easy it is to develop on the FoxBoard using NodeJS.

## A simple server side test for acme.gpio

With this first example <span class="github" user="ant9000" project="FoxNode" file="examples/test_gpio.js">test_gpio.js</span> we make use of two GPIO pins, namely J7.3 as input and J7.4 as output. Wire a push button between J7.3 and J7.40, and a led in series with a 1kOhm resistance between J7.4 and J7.1, then execute the program with

<pre class="minicom">
node test_gpio.js
</pre>

and leave it running. If you press the button, the program will log the state change to the console. Moreover, the app reacts to signals USR1 and USR2: if you open another connection to the FoxBoard, the command

<pre class="minicom">
killall -USR1 test_gpio.js
</pre>

will produce a dump of the current state and count for J7.3 and J7.4, while

<pre class="minicom">
killall -USR2 test_gpio.js
</pre>

will toggle the state of J7.4 - you will see the led switching on and off, and a console log will also inform you of the change.

## A server side test for acme.daisy

For trying this [sample](https://github.com/ant9000/FoxNode/tree/master/examples/test_daisy.js) you need a Daisy1 (the Daisy connector module), a Daisy5 (8 push buttons) and a Daisy11 (8 leds). Plug the Daisy1 onto your FoxBoard, then connect the Daisy5 to D5 and the Daisy11 to D2. You can then launch the program as

<pre class="minicom">
node test_daisy.js
</pre>

and leave it running. If you press any button on the Daisy5, the program will log the button state change to the console. Moreover, the app reacts to signals USR1 and USR2: if you open another connection to the FoxBoard, the command

<pre class="minicom">
killall -USR1 test_daisy.js
</pre>

will produce a dump of the current state and count for every button and led of the two Daisy boards, while

<pre class="minicom">
killall -USR2 test_daisy.js
</pre>

will toggle the state of L3 on Daisy11 - you will see the led switching on and off, and a console log will also inform you of the change.

## Web app sample for GPIO, using meryl

This [sample](https://github.com/ant9000/FoxNode/tree/master/examples/meryl_gpio/) uses the same setup as the server side GPIO test, i.e. a push button connected to J7.3 and a led wired to J7.4 - but this time the interaction with the users will not be on the FoxBoard console, but on a web page.

On first usage, type

<pre class="minicom">
npm bundle
</pre>

in the example directory to download and install the needed dependencies.

The example is executed as

<pre class="minicom">
./app.js
</pre>

Wait a few seconds for the *socket.io started* message to appear on console, then point your browser to port 8080 of the Fox and enjoy.

Push the button: the green led on the page will light up instantly. Click on the red led on the webpage, and the real led will react at once - retroacting a change on the page, too.

For more fun, point multiple browser windows to the same page and watch them as they synchronize in real time.

## Web app sample for Daisy, using express<

Before starting the [example](https://github.com/ant9000/FoxNode/tree/master/examples/express_daisy/), attach a Daisy5 to connector D5 on your Daisy1 and a Daisy11 to connector D2 - exactly as for the server side Daisy test.

On first usage, type

<pre class="minicom">
npm bundle
</pre>

in the example directory to download and install the needed dependencies.

The example is executed as

<pre class="minicom">
./app.js
</pre>

Wait a few seconds for the *socket.io started* message to appear on console, then point your browser to port 3000 of the Fox and enjoy.

Push any button on your Daisy5: the corresponding green led on the page will light up instantly. Click any of the red leds on the webpage, and the Daisy11 leds will react immediately - retroacting a change on the page, too.

For more fun, point multiple browser windows to the same page and watch them as they synchronize in real time.

