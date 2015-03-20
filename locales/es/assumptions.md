# Verificando datos

El procesamiento de datos es un negocio tramposo, lleno de dificultades y trampas. Esperemos que las tareas de esta guía ayudem a iniciarse en este proceso. Pero tu, yo, y el mundo entero cometerá errores. Es natural.

Pero errores en procesamiento de datos, como todos los otros tipos de errores, pueden ser dolorosos. Pueden resultar en horas buscando errores, días de reprocesamiento, y meses de llanto. Cómo nosotros sabemos que los errores suceden y continuarán sucediendo, ¿qué podemos hacer para quitarnos algo del dolor?

En una palabra, protección. Nosotros necesitamos alguna forma de protejernos de los golpes y moretones del procesamiento de datos. Y yo prodría sugerir que esa protección viene en forma de test simples que comprueban los supuesto que tienes sobre la forma y el contenido de tus datos.

Amenos que haya una necesidad de performance extrema, esos test deben ejecutarse en el pipeline del procesamiento de los datos. Optimamente, deben ser fáciles de encender y apagar para que puedas deshabilitarlos si necesitas implementar tu código.

## Aserciónes

Esos tests pueden ser creados con [aserciones](http://es.wikipedia.org/wiki/Aserci%C3%B3n_%28inform%C3%A1tica%29) - funciones que controla la veracidad de una sentencia en el código. Tipicamente, generan un error cuando un valor esperado no es verdadero.

Javascript no posee aserciones, pero podemos corregir esta deficiencia con una simple función.

@@ code=assumptions/assumptions.01.js @@

Esto emitirá un mensaje si la entrada no es verdadera. Tipicamente las aseciones [lanzan](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Sentencias/throw) errores, pero nosotros solo registraremos estos para propositos explicativos.

## Suposiciones en los contenidos de los datos

Ahora usemos nuestra función `assert` para verificar algunas suposiciones sobre los detalles de nuestros datos.

Podemos usar el juego de [funciones de checkeo de tipos](https://lodash.com/docs#isBoolean) de lodash para encargarnos de realizar las verificaciones, pasando el resultado de la verificación a `assert` para producir nuestros errores.

Digamos que nuestro proceso de importación de datos produjo algunos errores:

@@ code=assumptions/assumptions.02.js @@

El primer item se vee bien, pero nuestro segundo item tiene algunos problemas. El análisis de la edad para el inmortal [Sleepwalker](http://es.wikipedia.org/wiki/Sleepwalkers) lo ha dejado sin edad. También, malos datos de entrada ha no ha dejado con un string en `superhuman`, donde esperamos un booleano.

Una simple funcion de verificación de suposiciones que puede ejecutarse en estos datos puede lucir similar a esto:

@@ code=assumptions/assumptions.03.js @@

@@ code=assumptions/assumptions.03.out @@

<div class="aside">Este código esta usando lodash</div>

De nuevo, el foco aquí es la detección de problema en los datos. Tu quieres algo rápido y simple que sirva como una señal de alerta temprana.

Desafortunadamente, la primitiva de JavaScript `NaN` es en efecto un número, y entonces es necesario realizar verificaciones adicionales. A medida que más datos ingresan, esta función necesitará ser actualizada para añadir más verificaciones. Esto puede resultar un poco tedioso, pero un poco de verificaciones puede resultar en un largo camino hacia mantener la cordura.

## Suposiciones en la forma de los datos

Así cómo puedes probar tus suposiciones sobre el contenido de tus datos, puede ser una buena idea probar tus suposiciones sobre la _forma_ de tus datos. Aquí, la forma solo se refiere al tamaño y la estructura de tus datos. Filas y columnas.

Algo simple para realizar esta verificación puede lucir cómo esto:

@@ code=assumptions/assumptions.04.js @@

@@ code=assumptions/assumptions.04.out @@

Las dos funciones pueden combinarse fácilmente en una, pero es importante prestar atención a ambos aspectos de tus datos.

## Más aserciones

Si esto es un enfoque que te atrae, y tus datos pueden volverse realmente complicados (o realmente confusos), tu puedes querer esporar usando código de aserción más complicado.

Una librería muy útil para explorar es [Chai](http://chaijs.com/api/assert/), la cuál viene con una gran colección de funciones de aserción. Eso puede ayurate a verificar cosas más complicadas cómo cuándo dos objetos son iguales o cuando un objeto tiene o no una propiedad.

Por ejemplo:

@@ code=assumptions/assumptions.05.js @@

@@ code=assumptions/assumptions.05.out @@

<div class="aside">Este código esta usando la librería de aserciones de chai</div>

## Próxima tarea

[Usando Node](node.html)

## Ver también

- [Analizando datos crudos](http://www.pgbovine.net/parsing-raw-data.htm) - una gran guia que motivo esta sección
- [Chai](http://chaijs.com/api/assert/) - Librería de aserciones Chai
