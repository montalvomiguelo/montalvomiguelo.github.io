---
title: "Node.js Básico"
categories: blog
tags: Javascript
---
Para usar Node.js localmente en nuestra computadora necesitamos
descargarlo de [https:/nodejs.org/](https://nodejs.org/en/)

Node.js es una aplicación que se ejecuta desde la terminal,
entonces abrimos nuestra terminal y escribimos el siguiente comando

```shell
$ node -v
```

La salida de ese comando nos mostrará la versión de Node.js que tenemos
instalado actualmente en el equipo
```shell
v8.11.1
```

## Consola interactiva de Node.js
Cuando uno ejecuta el comando `node` solito, Node.js nos mostrará la
consola interactiva donde podemos ejecutar cualquier expresión de
JavaScript. A esta consola se le conoce como REPL *(read evaluate print
loop)*

```shell
$ node
> 2 + 2
4
```

Hay dos maneras de salir de la consola interactiva de Node.js

* Presionando `Ctrl + d` una vez
* Presionando `Ctrl + c` dos veces.

## Hola mundo!
Creamos un archivo llamado `app.js` con el siguiente contenido:

{% highlight javascript %}
console.log('Hola mundo!');
{% endhighlight %}

Para ejecutar el programa escribimos en la terminal `node` y el nombre
del archivo

```
$ node app.js
```

Nuestro programa entonces imprime lo siguiente
```
Hola mundo!
```

## JavaScript fuera del Navegador
Tradicionalmente uno escribe código JavaScript que será interpretado por
el motor de algún Navegador (Chrome, Firefox, etc) donde tenemos
disponibles dos tipos de objetos.

### Objetos Nativos *(Native Objects)*
Objetos que nos da JavaScript como lenguaje sin importar el entorno

* String
* Array
* Date
* Math

### Objetos de Entorno *(Host objects)*
En el caso del navegador disponemos de objetos como

* window
* document
* history
* XMLHttpRequest

En Node.js tenemos otro tipo de objetos disponibles como

* http
* process
* url
* fs

A esto se le conoce como Node.js environment o entorno. Como
desarrolladores de JavaScript es importante saber que está disponible
para nosotros dependiendo del entorno donde estemos programando.

## Documentación de Node.js
Uno como desarrollador frecuentemente necesita revisar la documentación
de la tecnología que está utilizando para saber que tipo de
funcionalidades están disponibles para nosotros y como utilizarlas.

En el sitio [https://nodejs.org/en/docs/](https://nodejs.org/en/docs/)
podemos consultar la documentación de Node.js dependiendo de la versión
que tengamos instalada.

Por ejemplo [V8.11.1 API LTS](https://nodejs.org/dist/latest-v8.x/docs/api/)
pero antes de todo es importante saber que existen indices de estabilidad
 de las funcionalidades o APIs que nos ofrece Node.js

En la sección [About these
Docs](https://nodejs.org/dist/latest-v8.x/docs/api/documentation.html)
podemos ver los siguientes índices

* 0 - Deprecates
* 1 - Experimental
* 2 - Stable
* 3 - Locked

Donde siempre trataremos de elegir APIs que tengan indice 2 o 3. Por ejemplo la funcionalidad
[Console](https://nodejs.org/dist/latest-v8.x/docs/api/console.html)
tiene el indice de estabilidad 2 y es seguro de utilizar.

## Eventos
En JavaScript nosotros podemos usar funciones (callback functions) para hacer algo cuando un
evento ocurre.

Existen dos tipos de eventos

### User events
Cuando el evento es provocado por el usuario, ejemplo cuando el usuario
hace click en un botón

{% highlight javascript %}
button.addEventListener('click', () => console.log('clicked'));
{% endhighlight %}

### System events
Cuando el evento es provocado por el sistema, ejemplo cuando usamos la
función setTimeout

{% highlight javascript %}
setTimeout(() => console.log('Boom!'), 3000);
{% endhighlight %}

Node.js tiene varios tipos de eventos, algunos de ellos son los
siguientes:

* Data events - Como cuando tratamos de leer data de una URL
* Completion events - Cuando una acción se ejecutó correctamente
* Error events - Cuando un error ocurrió durante la ejecución de una
  acción

## Creando una App para consultar el tipo de cambio de una moneda
Utilizaremos el API de
[https://exchangeratesapi.io](https://exchangeratesapi.io/) para
consultar el tipo de cambio de una moneda.

Al ser https usaremos el módulo de Node.js
[https](https://nodejs.org/dist/latest-v8.x/docs/api/https.html) para
hacer la petición. Entonces revisamos la documentación para ver como se
utiliza.

La respuesta de la petición tiene los eventos `data` y `end` donde
usaremos unos callbacks para hacer algo con la información que nos está
llegando.

Con ayuda de el objeto nativo de JavaScript `JSON` vamos a parsear la
información para mostrarla al usuario en formato json.

{% highlight javascript %}
// Necesitamos del módulo https
const https = require('https');

// Tomamos el base currency de la lista de argumentos al ejecutar la app
const base = process.argv[2];

// Hacemos una peticion get al API de exchangeratesapi.io
https.get('https://exchangeratesapi.io/api/latest?base=' + base, (res) => {
  let rawData = '';
  // Cuando recibimos data, vamos concatenando los fragmentos de información que nos llegan
  res.on('data', (chunk) => rawData += chunk);
  // Cuando terminó la petición, imprimimos la data en formato JSON
  res.on('end', () => {
    try {
      const parsedData = JSON.parse(rawData);
      console.dir(parsedData);
    } catch(e) {
      console.error(e.message);
    }
  });
  // Cuando hubo un error, mostramos el error
}).on('error', (e) => console.error(e.message));
{% endhighlight %}

Ejecutamos el programa

```bash
$ node exchangerates.js CAD
```
