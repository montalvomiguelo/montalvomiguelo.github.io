---
title:  "Herencia en JavaScript"
categories: blog
tags: Javascript
---
## Compartir métodos entre diferentes objetos

Esta es una alternativa que tenemos para emular herencia en
JavaScript. Lo que hacemos acá es simplemente compartir los
métodos y propiedades de un objeto en otro.

{% highlight js %}
// Constructor Base
function Shape () {
  return {
    type: this.constructor.name,
    sides: []
  }
}

function Rectangle(width, height) {
  // Instancia del Constructor Base
  var shape = Shape.call(this);

  // Establecer propiedades
  shape.sides.push(width, height, width, height);
  shape.getArea = function() {
    return shape.sides[0] * shape.sides[1];
  }

  return shape;
}

function Square(width) {
  // Instancia del constructor Rectangle
  var rectangle = Rectangle.apply(this, [width, width]);

  return rectangle;
}

var myRectangle = new Rectangle(5, 7);
console.log(myRectangle.type, myRectangle.getArea()); // Rectangle 35

var mySquare = new Square(5);
console.log(mySquare.type, mySquare.getArea()); // Square 25
{% endhighlight %}

Es una manera simple de compartir código, pero realmente lo que está
pasando es que cada instancia del constructor `Rectangle` tiene su
propia copia del método `getArea`, jamás se heredan por medio de la cadena de `prototype`.

Además todos los objetos son instancias de `Object`, no de los
constructores como tal.

{% highlight js %}
console.log(mySquare.hasOwnProperty('getArea')); // true
console.log(mySquare instanceof Square); // false
console.log(mySquare instanceof Object); // true
{% endhighlight %}


## Herencia prototypal
Es la manera estándar que ofrece JavaScript para herencia de
propiedades y métodos. Debido a esto se dice que JavaScript es un
lenguaje orientado a Prototipos.

 {% highlight js %}
// Constructor Base
function Shape() {}

// Definición de métodos en prototype de Constructor Base
Shape.prototype.getArea = function() {
  return this.sides[0] * this.sides[1];
}

function Rectangle(width, height) {
  this.sides = [width, height, width, height];
}

// Heredar del prototype de Shape sus métodos y propiedades
Rectangle.prototype = new Shape();
// Mantener constructor
Rectangle.prototype.constructor = Rectangle;

function Square(width) {
  this.sides = [width, width, width, width];
}

Square.prototype = new Shape();
Square.prototype.constructor = Square;

var myRectangle = new Rectangle(5, 7);
console.log(myRectangle.constructor.name, myRectangle.getArea()); // Rectangle 35

var mySquare = new Square(5);
console.log(mySquare.constructor.name, mySquare.getArea()); // Square 25
 {% endhighlight %}

 La gran diferencia acá es que el método `getArea` quedó definido en el
 `prototype` del constructor base `Shape` y este se va
 heredando actualizando el `prototype` de `Rectangle` y de `Square`.
 Además todos los objetos son instancias de sus respectivos
 constructores:

{% highlight js %}
console.log(mySquare.hasOwnProperty('getArea')); // false
console.log(mySquare instanceof Square); // true
{% endhighlight %}

También es posible sobreescribir un método de una manera diferente
agregandolo como una propiedad "propia" del objeto en el constructor:

{% highlight js %}

function Triangle(base, height) {
  this.sides = [base, height];
  // Sobreescribir el método getArea
  this.getArea = function() {
    return .5 * this.sides[0] * this.sides[1];
  }
}

Triangle.prototype = new Shape();
Triangle.prototype.constructor = Triangle;

var myTriangle = new Triangle(4, 10);

console.log(myTriangle.constructor.name, myTriangle.getArea()); // Triangle 20
console.log(myTriangle.hasOwnProperty('getArea')); // true
{% endhighlight %}

