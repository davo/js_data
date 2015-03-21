# Leyendo los datos

¡El primer paso en cualquier procesamiento de datos es obtener los datos! Aquí es cómo se analiza y prepara un formato de entrada común usando D3.js

## Analizando archivos CSV

[D3 tiene un montón](https://github.com/mbostock/d3/wiki/Requests) de tipos de archivos que soporta cuando se cargan los datos, y uno de los más comunes es el viejo CSV (valores separados por coma).

Vamos a decir que tienes un archivo csv con los datos de alguna ciudad:

```bash
cities.csv:

city,state,population,land area
seattle,WA,652405,83.9
new york,NY,8405837,302.6
boston,MA,645966,48.3
kansas city,MO,467007,315.0
```

Usa [d3.csv](https://github.com/mbostock/d3/wiki/CSV) para convertirlo en un array de objetos

@@ code=read_data/read_data.01.js @@
@@ code=read_data/read_data.01.out @@

<div class="aside">Este código está usando d3.js</div>

Puedes ver en los encabezados que el CSV original están siendo usado cómo los nombres de las propiedades de los objetos. Usando `d3.csv` de esta manera requiere que tu archivo CSV tenga un registro de encabezado.

Si miras de cerca, tambien podras ver que los valores asociados con esas propiedades son todos strings. Esto probablente _no es lo que quieras_ en el caso de números. Cuando cargamos CSVs y otros archivos planos, tu tienes que hacer la conversión de tipos.

Nosotros podremos ver más de esto en otras tareas, pero una forma simple de hacerlo es usar el operador [+](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Unary_plus) (suma unitaria). El `forEach` puede ser utilizado para iterar sobre el array de datos.

@@ code=read_data/read_data.02.js @@
@@ code=read_data/read_data.02.out @@

<div class="aside">Este código está usando d3.js</div>

La [notación por punto](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Operadores/Miembros) es una forma útil para acceder alas propiedades de esos objetos. Sin embargo, si tus encabezados tienen espacios en ellos, necesitarás usar la notación por corchetes cómo se muestra.

Esto puede tambien ser hecho usando la carga de los datos directamente con `d3.csv`. Esto es hecho proveyendo la funcion de acceso a `d3.csv`, cuyo valor de retorno serán los objectos individuales en nuestro array de datos.

@@ code=read_data/read_data.03.js @@
@@ code=read_data/read_data.03.out @@

<div class="aside">Este código está usando d3.js</div>

De esta forma, tienes control completo sobre los objetos y puedes renombrar propiedades (cómo `land_area`) y convertir valores (cómo `population`). Por otro lado, tienes que ser bastante explicito sobre que valores retornar. Esto puede o no ser lo que quieras.

Yo tipicamente permito a D3 cargar todos los datos, y despues hacer modificaciones en un paso post-procesamiento, pero esto puede ser más efectivo para ti para ser más explícito con las modificaciones.

## Leyendo archivos TSV

CSV es probablemente el formato de archivo plano más común, pero de ninguna manera el único.

A menudo me gusta usar TSV (archivos separados por tabs) - para evitar problemas con números y strings que comúnmente tienen comas.

D3 puede analizar archivos TSV con [d3.tsv](https://github.com/mbostock/d3/wiki/CSV#tsv):

```
animals.tsv:

name	type	avg_weight
tiger	mammal	260
hippo	mammal	3400
komodo dragon	reptile	150
```
Cargando animals.tsv con `d3.tsv`:

@@ code=read_data/read_data.04.js @@
@@ code=read_data/read_data.04.out @@

<div class="aside">Este código está usando d3.js</div>

## Leyendo otros archivos planos

De hecho, `d3.tsv` y `d3.csv` son solo la punta del iceberg. Si tu tienes un archivo delimitado no estandard, puedes crear tu propio analizador facilmente utilizando [d3.dsv](https://github.com/mbostock/d3/wiki/CSV#arbitrary-delimiters)

Usando `d3.dsv` toma un paso más. Primero debes crear un nuevo analizador pasandole cómo parámetro el tipo de delimitador y [mimeType](http://en.wikipedia.org/wiki/Internet_media_type) a utilizar.

Por ejemplo, si tienes un archivo que luce cómo este:
```
animals_piped.txt:

name|type|avg_weight
tiger|mammal|260
hippo|mammal|3400
komodo dragon|reptile|150
```

Podemos crear un analizador de archivos separados por pipes (PSV) usando `d3.dsv`:

@@ code=read_data/read_data.05.js @@

Y luego usarlo para analizar el archivo formateado extrañamente.

@@ code=read_data/read_data.06.js @@
@@ code=read_data/read_data.06.out @@

<div class="aside">Este código está usando d3.js</div>

## Leyendo archivos JSON

Para datos anidados, o para pasar datos donde no quieres jugar con tipos de datos, es dificil de vencer a [JSON](http://json.org/).

JSON se ha convertido el lenguaje de internet por una buena razón. Es fácil de entender, escribir y analizar. Y con [d3.json](https://github.com/mbostock/d3/wiki/Requests#d3_json) - tambien puedes aprovechar su poder.

```
employees.json:

[
 {"name":"Andy Hunt,
 "title":"Big Boss",
   "age": 68,
   "bonus": true
 },
 {"name":"Charles Mack",
  "title":"Jr Dev",
  "age":24,
  "bonus": false
 }
]
```

Cargando employees.json con `d3.json`:

@@ code=read_data/read_data.07.js @@
@@ code=read_data/read_data.07.out @@

<div class="aside">Este código está usando d3.js</div>

Podemos ver qué, a diferencia de nuestro analizador de archivos planos, los tipos numéricos permanecen numéricos. Así es, un valor de un JSON puede ser un string, un número, un valor booleano, un array u otro objeto. Esto permite anidar datos para ser tratados fácilmente.

## Cargando múltiples archivos

El sistema de carga básico de D3 está bien para un solo archivo, pero comienza a ser complicado cuando anidamos múltiples callbacks.

Para cargar múltiples archivos, podemos usar [Queue.js](https://github.com/mbostock/queue) (también escrito por Mike Bostock) para esperar múltiples orígenes de datos a ser cargados.

@@ code=read_data/read_data.08.js @@
@@ code=read_data/read_data.08.out @@

<div class="aside">Este código está usando queue.js y d3.js</div>

Nota quñe nosotros `desfazamos` ña carha de dos toipos de archivos - usando dos funciones de carga diferentes - de forma que es una formá simple de mezclar e igualar tipos de archivos.

La funcón de callback pasada a `await` obtiene cada set de datos cómo un parámetro, con el primer parámetro siendo populado si un error ha ocurrido mientra se cargan los datos.

Esto puede ser útil para imprimir el error, si esta definido, de forma de atrapar los problemas de carga de datos rápidamente.

¡Para agregar otro archivo de datos, simplemente agrega otro diferido y extiende los parámetros de entrada para tu callback!


## Siguiente tarea

[Resumiendo los datos](summarize_data.html)

## Ver también

- [Documentación de D3](https://github.com/mbostock/d3/wiki/Requests)
- [Cargando un XML con D3](https://github.com/mbostock/d3/wiki/Requests#d3_xml)
- [Cargando SVGs externos con D3](http://bl.ocks.org/mbostock/1014829) - ¡SVG es simplemente un XML!
