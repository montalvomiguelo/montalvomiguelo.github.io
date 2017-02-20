---
title:  "Esenciales de Flexbox"
categories: blog
tags: CSS
---
Flexbox es un conjunto de propiedades CSS que nos ayudan a crear
layouts de nuestras páginas web de una manera más sencilla y fácil de
entender sin necesidad de utilizar _floats_ o elementos _inline-block_. Con
pocas líneas de código podemos decidir el comportamiento de nuestros
elementos _(columnas, filas, dirección, alineación, tamaño, orden,
etc)_. A continuación estudiaremos las principales propiedades de
Flexbox.

## Contenedores Flexbox
Para empezar a trabajar con Flexbox necesitamos crear un contenedor con
la propiedad `display: flex`.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title></title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <div class="container">
    <div class="container__item bg--blue">Uno</div>
    <div class="container__item bg--red">Dos</div>
    <div class="container__item bg--purple">Tres</div>
    <div class="container__item bg--yellow">Cuatro</div>
  </div>
</body>
</html>
```

```css
.container {
  background: #ecf0f1;
  display: flex;
  height: 400px;
  margin-left: auto;
  margin-right: auto;
  width: 800px;
}

.bg--blue {
  background: #3498db;
}

.bg--red {
  background: #c0392b;
}

.bg--purple {
  background: #9b59b6;
}

.bg--yellow {
  background: #f1c40f;
}
```

Tradicionalmente sabemos que los elementos `div` de nuestra página
tienen un _display block_ por lo cual se acomodan uno debajo del otro.
Pero cuando nuestro contenedor tiene la propiedad `display: flex` sus
hijos directos obtienen propiedades de Flexbox y el primer
comportamiento que notarás es que estos se convierten en columnas, que ocupan
el ancho de acuerdo a su contenido dentro de ellos y que además ocupan
todo el alto disponible de su contenedor.

![Flex container]({{ site.baseurl }}/assets/display-flex.png)

## Ancho de columnas
Sabemos que los hijos directos de un contenedor con `display:
flex` por defecto se acomodarán en forma de columnas uno después de otro
y que su ancho queda definido por su contenido dentro de ellos.
Ahora veremos como establecer
el ancho de esos elementos con otras propiedades de Flexbox.

### flex-basis
Con la propiedad `flex-basis` podemos establecer el ancho inicial de los
elementos de un contenedor _flex_.

```css
.container__item {
  flex-basis: 150px;
}
```

Nuestro layout se ve algo así:

![Flex basis]({{ site.baseurl }}/assets/flex-basis.png)

## flex-grow
Ahora veremos como hacer crecer el ancho de nuestras columnas con la
propiedad `flex-grow` para que puedan llenar todo el ancho de su
contenedor:

```css
.container__item {
  flex-basis: 150px;
  flex-grow: 1;
}
```

![Flex grow]({{ site.baseurl }}/assets/flex-grow.png)

Lo que hizo `flex-grow: 1;` es hacer crecer todos los elementos de la
misma manera para que puedan llenar el espacio de su contenedor.

Veamos que pasa cuando queremos hacer crecer uno de los elementos
2 veces para entender como funciona _flex-grow_.

```css
.container__item:nth-child(2) {
  flex-grow: 2;
}
```

![Flex grow]({{ site.baseurl }}/assets/flex-grow-2.png)

Hacer crecer un elemento 2 veces con `flex-grow` no quiere decir que
este elemento va tener el doble de tamaño que sus hermanos. Lo que
quiere decir es que va tener 2 veces el crecimiento que tuvieron sus
hermanos en relación al ancho inicial que definimos anteriormente con
`flex-basis`.

Los elementos que hicimos crecer 1 vez con `flex-grow: 1;` tuvieron un
crecimiento de __+40px__ cuando el ancho inicial con `flex-basis` fue de
__150px__, dando un total de __190px__.

![grow 1]({{ site.baseurl }}/assets/grow-1.png)

El elemento que hicimos crecer 2 veces con `flex-grow: 2;` entonces
creció dos veces más que sus hermanos __+80px__, dando un total de
__230px__.

![grow 2]({{ site.baseurl }}/assets/grow-2.png)

### flex-shrink

Con esta propiedad podemos encoger el ancho de nuestras columnas, justo
lo opuesto de _flex-grow_ aunque funcionan de manera similar:

```css
.container__item {
  flex-basis: 400px;
  flex-shrink: 1;
}

.container__item:nth-child(2) {
  flex-shrink: 2;
}
```

![Flex grow]({{ site.baseurl }}/assets/flex-shrink.png)

Todos los elementos tienen un ancho inicial de __400px__ establecido
con `flex-basis`. Los elementos que se encogieron 1 vez con
`flex-shrink: 1;` redujeron su tamaño __-160px__, dando un total de
__240px__.

![shrink 1]({{ site.baseurl }}/assets/shrink-1.png)

El segundo elemento que hicimos encoger su ancho 2 veces más que sus
hermanos entonces se redujo __-320px__, dando un ancho total de
__80px__.

![shrink 2]({{ site.baseurl }}/assets/shrink-2.png)

### flex
La propiedad _flex_ es un shorthand para establecer los siguientes
valores.

* flex-grow
* flex-shrink
* flex-basis

```css
.container__item {
  flex: 1 1 400px;
}

.container__item:nth-child(2) {
  flex: 1 2 400px;
}
```

![Flex grow]({{ site.baseurl }}/assets/flex-shrink.png)

Si queremos podemos pasar únicamente el primer valor,  y veamos que es
lo que pasa:

```css
.container__item {
  flex: 1;
}

.container__item:nth-child(2) {
  flex: 2;
}
```

![Flex grow]({{ site.baseurl }}/assets/flex.png)

Como podemos observar pasando solo el primer valor podemos hacer
fácilmente que uno de los elementos sea del doble de ancho que sus
hermanos.

## Filas o Columnas
Anteriormente vimos que por defecto los elementos dentro de nuestro
contenedor _flex_ se acomodaban de manera horizontal uno después del
otro. Pero podemos decidir ese comportamiento con la propiedad
`flex-direction`.

### flex-direction
Esta propiedad la escribiremos en nuestro contenedor _flex_. El valor
por defecto que este tiene es `flex-direction: row;`.

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-direction: row;
  height: 400px;
  margin-left: auto;
  margin-right: auto;
  width: 800px;
}
```

![Flex grow]({{ site.baseurl }}/assets/flex.png)

Podemos hacer que los elementos se acomoden de manera vertical uno
debajo de otro con `flex-direction: column;`.

![Flex column]({{ site.baseurl }}/assets/flex-column.png)

También podemos hacer que los elementos se acomoden al revés con los
siguientes valores:

* `flex-direction: row-reverse;`
* `flex-direction: column-reverse;`

![column reverse]({{ site.baseurl }}/assets/column-reverse.png)

## Orden de los elementos
Con Flexbox podemos establecer el orden de los elementos de un
contenedor _flex_. Para ello utilizamos la propiedad `order` en cada uno
de los elementos.

## order
Establece el orden del elemento dentro de el contenedor _flex_.

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-direction: row;
  height: 400px;
  margin-left: auto;
  margin-right: auto;
  width: 800px;
}

.container__item {
  flex: 1;
}

.container__item:nth-child(2) {
  order: 1;
}

.container__item:first-child {
  order: 2;
}

.container__item:nth-child(3) {
  order: 4;
}

.container__item:last-child {
  order: 3;
}
```

![Order]({{ site.baseurl }}/assets/order.png)

## Ajuste de elementos
Para ver otras propiedades de Flexbox vamos a actualizar un poco el
contenido de nuestro documento HTML.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title></title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <div class="container">
    <div class="container__item bg--blue">
      <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Donec odio. Quisque volutpat mattis eros. Nullam malesuada erat ut turpis. Suspendisse urna nibh, viverra non, semper suscipit, posuere a, pede.</p>
    </div>
    <div class="container__item bg--red">
      <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit.</p>
    </div>
    <div class="container__item bg--purple">
      <p>Donec nec justo eget felis facilisis fermentum. Aliquam porttitor mauris sit amet orci. Aenean dignissim pellentesque felis.</p>
    </div>
    <div class="container__item bg--yellow">
      <p>Integer vitae libero ac risus egestas placerat.</p>
    </div>
  </div>
</body>
</html>
```

Asignamos un ancho máximo a nuestro contenedor de 800px y eliminamos el
alto.

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-direction: row;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
}

.container__item {
  flex: 1;
}
```

Entonces si redimensionamos nuestro navegador veremos que tenemos
elementos que se ajustan de manera automática al ancho que llegue a
tener su contenedor.

![Fluid]({{ site.baseurl }}/assets/fluid.png)

Pero nosotros queremos establecer un ancho mínimo pues no queremos que
todo quede tan apretado.

```css
.container__item {
  flex: 1;
  min-width: 150px;
}
```

![Overflow]({{ site.baseurl }}/assets/overflow.png)

Entonces para evitar que se salgan los elementos del contenedor usaremos
la propiedad `flex-wrap`.

### flex-wrap
Con esta propiedad podemos hacer que los elementos del contenedor _flex_
se ajusten de diferentes maneras. Por defecto _nowrap_.

* nowrap
* wrap
* wrap-reverse

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
}
```

![Wrap]({{ site.baseurl }}/assets/wrap.png)

Con `flex-wrap: wrap;` los elementos que ya no caben se acomodan en la
siguiente línea.

### flex-flow
Esta propiedad es un shorthand para establecer las siguientes
propiedades de una sola vez:

* flext-direction
* flex-wrap

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-flow: row wrap;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
}
````

## Main Axis y Cross Axis
En Flexbox existen dos ejes en los cuales los elementos _flex_ son
colocados:

### Main Axis
Es el eje principal donde los elementos _flex_ se van posicionado uno
después de otro.

### Cross Axis
Es el eje perpendicular al Main Axis

Veamos siguiente ejemplo para describir los ejes:

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-flow: row wrap;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
  min-height: 494px;
}

.container__item {
  flex-basis: 150px;
  height: 200px;
}
```

![Axis]({{ site.baseurl }}/assets/main-cross-axis.png)

Una vez que conocemos estos ejes vamos a ver como podemos alinear
nuestros elementos en base a estos.

## Alineación en Cross Axis
Primero hagamos algunos ajustes para establecer un ancho inicial de
nuestros elementos de 400px.

```css
.container__item {
  flex-basis: 400px;
  flex-grow: 1;
  height: 200px;
}
```

![Cross Axis]({{ site.baseurl }}/assets/cross-axis-0.png)

Al hacer estos nuestros elementos se van desbordando hacia la
siguiente línea sobre el Cross Axis.

### align-content

Esta propiedad nos ayuda a alinear nuestros elementos sobre el Cross
Axis y puede recibir multiples valores.

* flex-start
* flex-end
* center
* space-between
* space-around
* space-evenly
* stretch

Ejemplo con `align-content: flex-start;`

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-flow: row wrap;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
  min-height: 494px;
  align-content: flex-start;
}
```

![Align Content Start]({{ site.baseurl }}/assets/align-content-start.png)

Ejemplo con `align-content: center;`

![Align Content Center]({{ site.baseurl }}/assets/align-content-center.png)

__Nota:__ Únicamente funciona cuando hay
elementos que se desbordan en otra línea sobre el Cross Axis y solo
sobre esas líneas.

### align-items
Esta propiedad alinea cada uno de los elementos _flex_ sobre el Cross
Axis, por defecto tiene un valor de `stretch` que hace que estos ocupen
todo el alto de su contenedor, pero puede recibir los siguientes
valores.

* flex-start
* flex-end
* center
* baseline
* stretch

Ejemplo con `align-items: center;`

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-flow: row wrap;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
  min-height: 494px;
  align-items: center;
}

.container__item {
  flex-basis: 150px;
  flex-grow: 1;
}
```

![Align Items Center]({{ site.baseurl }}/assets/align-items-center.png)

### align-self
También es posible alinear cada uno de los elementos de manera
individual. La propiedad `align-self` es aplicada a los elementos
_flex_ y puede recibir los mismos valores que la propiedad
`align-items`.

Ejemplo con `align-self: flex-start;`

```css
.container__item:last-child {
  align-self: flex-start;
}
```

![Align Self]({{ site.baseurl }}/assets/align-self.png)

## Alineación en Main Axis
Ahora veamos como podemos alinear nuestros elementos sobre el eje
principal.

### justify-content
Esta es la propiedad que nos permite alinear nuestros elementos en el
Main Axis y puede recibir los siguientes valores.

* flex-start
* flex-end
* center
* space-between
* space-around

Ejemplo con `justify-content: space-between;`

```css
.container {
  background: #ecf0f1;
  display: flex;
  flex-flow: row wrap;
  margin-left: auto;
  margin-right: auto;
  max-width: 800px;
  min-height: 494px;
  justify-content: space-between;
}

.container__item {
  flex-basis: 150px;
}
```

![Justify Content Space Betweeen]({{ site.baseurl }}/assets/justify-content-space-between.png)

## Prefijos propietarios
Flexbox no funciona por defecto en todos los navegadores, debemos
agregar prefijos propietarios.

[http://caniuse.com/#feat=flexbox](http://caniuse.com/#feat=flexbox)

```css
#flex-container > .flex-item {
  -webkit-flex: auto;
  flex: auto;
}
```
