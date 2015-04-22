# Trabajando con tiempo

El tiempo es una de esas cosas complicadas de la programación que parece fácil, pero usualmente termina no siendolo. Vamos a utilizar las funciones [time formating](https://github.com/mbostock/d3/wiki/Time-Formatting) e [interval](https://github.com/mbostock/d3/wiki/Time-Intervals) construídas dentro de D3. También vamos a repasar la poderosa librería [Moment.js](http://momentjs.com/), para cuando necesitemos más poder sobre el tiempo.

## Cadena a fecha

La primera tarea cuando trabajamos con fechas es usualmnte obtener un [objeto Fecha](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) dentro de una cadena. La mayoría de las veces, sus datos poseen fechas o tiempo en un formato (mayormente) arbitrario, y usted necesita forzar ese desorden en una fecha puntual.

D3 tiene [d3.time.format](https://github.com/mbostock/d3/wiki/Time-Formatting#format) el cual provee una manera de interpretar esto. Fue un poco confuso para mí la primera vez que lo tuve que probar. Usted utilizará esta función para crear un interprete de una cadena, y luego utilizar el intérprete para finalmente convertir la cadena.

En nuestro [ejemplo de anidación](group_data.html), hemos visto datos que contienen fechas como cadenas:

@@ code=time/time.01.js @@

Para convertir esta cadena de fecha en un objeto Fecha, necesitaremos un interprete que se vea del siguiente modo:

@@ code=time/time.02.js @@
<div class="aside">Este código está utilizando d3.js</div>


El registro de cadena `d3.time.format` indica que como debería verse la cadena de fecha. Usted tiene [muchas opciones](https://github.com/mbostock/d3/wiki/Time-Formatting#format) for the special, percent-sign-prefixed variables. You can see in the string I'm using month, day, and four-digit year. The slashes in the format string are not special variables - but just what we expect to find separating the fields in the date string.

Next we use the parser to parse our string.

@@ code=time/time.03.js @@

@@ code=time/time.03.out @@

Cool! Now our date is actually a Date object.

Here are a few more time parsers to show the capabilities of D3's parsing.

Just the date:

@@ code=time/time.04.js @@

@@ code=time/time.04.out @@

<div class="aside">This code is using d3.js</div>

The little dash in front of the `d` is to remove the 0-padding)

@@ code=time/time.05.js @@

@@ code=time/time.05.out @@

You can see it defaults to the first day of the month.

Just the time:

@@ code=time/time.06.js @@

@@ code=time/time.06.out @@

<div class="aside">This code is using d3.js</div>

Gives you a somewhat strange default date.

Date and time:

@@ code=time/time.07.js @@

@@ code=time/time.07.out @@

<div class="aside">This code is using d3.js</div>

This could also be done using some built in short-hands:

@@ code=time/time.08.js @@

@@ code=time/time.08.out @@

<div class="aside">This code is using d3.js</div>

You can see that `d3.time.format` gives you a lot of flexibility about what your time string will look like.

## Modifying Time

In many cases, you might want to modify a date object. Perhaps you only want to display the hour from a date, or maybe you want to figure out what a week from now would be.

The [d3.time.interval](https://github.com/mbostock/d3/wiki/Time-Intervals) set of functions provides a starting point for these kinds of manipulations.

Intervals allow for modifying dates around specific time slices like minutes, hours, days, months, or years. We are given a number of functions to work with each interval, depending on what we might want to do.

So, to get the nearest hour from a date, we can use [d3.time.hour.round](https://github.com/mbostock/d3/wiki/Time-Intervals#interval_round)

@@ code=time/time.09.js @@

@@ code=time/time.09.out @@

<div class="aside">This code is using d3.js</div>

It returns a date object that just contains the nearest hour (11:00pm). We can display this by using the `d3.time.format` parser to format the date object into a string (these formaters can work both ways).

@@ code=time/time.10.js @@

@@ code=time/time.10.out @@

## Moment.js

[Moment.js](http://momentjs.com/) is another JavaScript library that could be better suited to your needs, if you happen to be doing a lot of time manipulations. Its syntax and capabilities seem a bit more intuitive for certain [time manipulations](http://momentjs.com/docs/#/manipulating/).

Reviselo si necesita más poder para controlar tiempo!

## Siguiente Tarea

[Checking Data Assumptions](assumptions.html)

## Ver También

- [moment.js](http://momentjs.com/)

