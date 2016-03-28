---
title:  "Métodos: call, apply & bind"
categories: blog
---
Las funciones en JavaScript también son objetos, tienen
propiedades y métodos. Algunos de los métodos más comunes son:

* call
* bind
* apply

## call
Permite invocar una función con un valor `this` asignado:

{% highlight js %}
var square = {
  length: 2
}

function getSquarePerimeter() {
  console.log('Perimeter: ' + this.length * 4);
}

getSquarePerimeter.call(square); // Perimeter: 8
{% endhighlight %}

El método call puede aceptar cualquier número de argumentos adicionales
que serán pasados a la función que está siendo invocada

{% highlight js %}
var square = {
  length: 2
}

function getSquarePerimeter(color) {
  console.log('Perimeter: ' + this.length * 4 + ' Color: ' + color);
}

getSquarePerimeter.call(square, 'blue'); // Perimeter: 8 Color: blue
{% endhighlight %}

## apply
Funciona de manera similar a `call`. La diferencia es que en lugar de
aceptar los argumentos adicionales uno por uno, basta con mandarle un
arreglo con todos ellos:

{% highlight js %}
function shapeStyle(fillColor, borderColor) {
  this.fillColor = fillColor;
  this.borderColor = borderColor;
}

shapeStyle.apply(square, ['blue', 'black']);

console.log(square); // Object {length: 2, fillColor: "blue", borderColor: "black"}
{% endhighlight %}

## bind
Este método nos retorna una nueva función con el valor `this` asignado:

{% highlight js %}
var mySquare = getSquarePerimeter.bind(square);

mySquare('yellow'); // Perimeter: 8 Color: yellow
{% endhighlight %}
