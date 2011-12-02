# The FoxNode server-side API

FoxNode aims at simplifying access to your hardware resources from within NodeJS. The library innermost part is [acme.gpio](https://github.com/ant9000/FoxNode/tree/master/acme/gpio/), a thin wrapper around the Linux kernel [GPIO](http://www.kernel.org/doc/Documentation/gpio.txt) sys interface. A C helper program for dispatching user space interrupts to NodeJS is also included: your code can react immediately to state changes, without wasting CPU cycles in querying the hardware.

The higher level [acme.daisy](https://github.com/ant9000/FoxNode/tree/master/daisy/) interface builds on these facilities to offer a simple programmatic access to Daisy devices. For now, only Daisy5 and Daisy11 are implemented, but support for more peripherals is planned.

-  [GPIO](?id=nodejs_api#GPIO)
-  [Daisy5](?id=nodejs_api#Daisy5)
-  [Daisy11](?id=nodejs_api#Daisy11)

<a name="GPIO"></a>
## The API for GPIO

First step, create a new GPIO pin object:

<pre class="prettyprint">
var acme = require('acme'),
    pin  = new acme.gpio.GPIO(name,direction,value);
</pre>

where

- *name* is any of the Fox/Daisy connectors, i.e. *'J6.1'*, *'J7.1'*, *'D1.1'*, ..., *'D8.8'*, or a kernel id number
- *direction* is one of *'in'* or *'out'*
- *value* can be *0* or *1*

Direction and value can be read and changed after initialization with the following methods:

<pre class="prettyprint">
pin.direction()           # get
pin.direction(direction)  # set

pin.value()               # get
pin.value(value)          # set
</pre>

Upon creation, the pin initializes the underlying hardware in order to react to state changes via interrupts. NodeJS is informed of such changes using a *'data'* event, that can be used like this:

<pre class="prettyprint">
pin.on('data',function(data){
  console.log(
    'Pin:   '+data.name +', '+
    'Value: '+data.value+', '+
    'Count: '+data.count
  );
});
</pre>

There is also a read-only property *pin.count* representing the number of value changes since pin instantiation.

It is possible to stop the event generation with 

<pre class="prettyprint">
pin.pause();
</pre>

and to restart it afterwards with

<pre class="prettyprint">
pin.resume();
</pre>

<a name="Daisy5"></a>
## The API for Daisy5

In order to instantiate a new Daisy5, the code is

<pre class="prettyprint">
var acme = require('acme'),
    daisy5 = new acme.daisy.Daisy5(port);
</pre>

where *port* is one of *'D2'* or *'D5'*.

Daisy5 is configured as 8 input pins, readable as

<pre class="prettyprint">
daisy5.P1
daisy5.P2
...
daisy5.P8
</pre>

and also as

<pre class="prettyprint">
daisy5.state(btn)
</pre>

where *btn* is one of *'P1'* ... *'P8'*.

Whenever one of the buttons changes state, the object will emit a *'data'* event:

<pre class="prettyprint">
daisy5.on('data',function(data){
  console.log(
    'Port:   '+data.port+', '+
    'Button: '+data.button+', '+
    'Value:  '+data.value+', '+
    'Count:  '+data.count
  );
});
</pre>

<a name="Daisy11"></a>
## The API for Daisy11

In order to instantiate a new Daisy11, the code is

<pre class="prettyprint">
var acme = require('acme'),
    daisy11 = new acme.daisy.Daisy11(port);
</pre>

where *port* is one of *'D2'* or *'D5'*.

Daisy11 is configured as 8 output pins, readable and writable as

<pre class="prettyprint">
daisy11.L1
daisy11.L2
...
daisy11.L8
</pre>

and also as

<pre class="prettyprint">
daisy11.state(led)
daisy11.state(led,value)
</pre>

where *led* is one of *'L1'* ... *'L8'*.

Whenever one of the leds changes state, the object will emit a *'data'* event:

<pre class="prettyprint">
daisy11.on('data',function(data){
  console.log(
    'Port:  '+data.port+', '+
    'Led:   '+data.led+', '+
    'Value: '+data.value+', '+
    'Count: '+data.count
  );
});
</pre>


