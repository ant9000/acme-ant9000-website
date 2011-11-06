### FoxNode Web page widgets ###

With the intent of making my [examples](/ant9000/FoxNode/tree/master/examples/) nicer, I wrote a few Javascript widgets that you can find in the [media](/ant9000/FoxNode/tree/master/media/js) folder of FoxNode. 

Since they could be useful for their own sake, I will try to present them here in some detail:

*  [LED](#led)
*  [7-segment display](#lcd)
*  [Daisy5 and Daisy11](#daisy)

<a name="led">LED</a>
---------------------

The LED widget can be used as a simple state indicator, reflecting the state of a hardware line on your FoxBoard. To use it, the only requirement is that the DOM should be available - no Javascript frameworks are required: simply include

```html
<link rel="stylesheet" href="/css/led.css" type="text/css" />
<script src="/js/led.js"></script>
```

in the head of your page, adjusting the paths accordingly, of course.

To instantiate a led named *myLed*, use

```javascript
var myLed = new LED(elem,state,color);
```

where *elem* is the id of a DOM element in the page, *state* is one of *0* or *1*, and *color* can be *'red'* or *'green'*. To modify the appearance of *myLed* you have a few available methods:


```javascript
myLed.on();
myLed.off();
myLed.toogle();
```

change the led state, while


```javascript
myLed.red();
myLed.green();
myLed.colorcycle();
```

do the same for led color. You can also modify both properties at once, with

```javascript
myLed.set(state,color);
```

(all of the other shortcuts are implemented as *.set()* calls, as you can imagine).


<a name="lcd">7-segment display</a>
-----------------------------------

The LCD widget is meant for showing a counter as a 7-segment display, adding some eye-candy to your pages. As for the LED, to use it you need the DOM available, but no Javascript framework is required: simply include

```html
<link rel="stylesheet" href="/css/lcd.css" type="text/css" />
<script src="/js/lcd.js"></script>
```

in the head of your page, adjusting the paths accordingly, of course.

To instantiate a LCD named *myLcd*, use

```javascript
var myLcd = new LCD(elem,digits,padding);
```

where *elem* is the id of a DOM element in the page and *digits* is the number of ciphers you want to use. The parameter *padding*, which is optional, can be used to right-align the display: it will contain the padding character to fill the LCD with.

In order to show something on the LCD, simply use

```javascript
myLcd.display(str);
```

where *str* should be an hexadecimal string.


<a name="daisy">Daisy5 and Daisy11</a>
--------------------------------------

I have decided to implement a view for Daisy5 (8 push buttons) and Daisy11 (8 leds) as a jQuery plugin. Usage is extremely simple:
include both jQuery and the plugin as

```html
<link rel="stylesheet" href="/css/jquery-daisy.css" type="text/css" />
<script src="/js/jquery.min.js"></script>
<script src="/js/jquery-daisy.js"></script>
```

at the top of your HTML page. You can then instantiate the object as

```javascript
var daisy5  = $(elem1).Daisy5();
var daisy11 = $(elem2).Daisy11();
```

where *elem1* and *elem2* can be any valid jQuery selector. In order to set the state of a button (for Daisy5) or led (for Daisy11), the code needed is

```javascript
daisy5.Daisy5('state',button,value);
daisy11.Daisy11('state',led,value);
```

where *button* is one of *'P1'*, ..., *'P8'*, *led* one of *'L1'*, ..., *'L8'* and *value* can be *0* or *1*.

Since Daisy11 are outputs, you might want the user to interact with the widget. When somebody clicks on it, it will generate a jQuery custom event named *daisy11*, that you can bind to with

```javascript
daisy11.bind('daisy11',function(event,data){ 
});
```

Apart from the usual *event* parameter, your callback receives an array *data* with the keys

```javascript
{ led: ..., value: ... }
```

where *led* can be any of *'L1'*, ..., *'L8'*, and *value* is *0* or *1*.
