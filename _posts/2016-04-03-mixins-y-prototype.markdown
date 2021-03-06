---
title:  "Mixins y Prototype"
categories: blog
tags: Javascript
---
Es la manera en la cual nosotros heredamos en un objeto las propiedades
y métodos de uno, o más prototipos.

{% highlight js %}
function mixin() {
  var property;
  var newPrototype = {};
  var argsArr = Array.prototype.slice.call(arguments);
  var baseObject = argsArr[0];
  var prototypes = argsArr.slice(1);

  prototypes.forEach(function(item) {
    for (property in item) {
      if (item.hasOwnProperty(property)) {
        newPrototype[property] = item[property];
      }
    }
  });

  baseObject.prototype = newPrototype;
  baseObject.prototype.constructor = baseObject;
}

function Shape() {}
Shape.prototype.getArea = function() {
  return this.sides[0] * this.sides[1];
}

function Style() {}
Style.prototype.fill = function() {
  return 'This ' + this.constructor.name + ' is ' + this.color;
}

function Rectangle(width, height, color) {
  this.sides = [width, height, width, height];
  this.color = color;
}

mixin(Rectangle, Shape.prototype, Style.prototype);

var myRectangle = new Rectangle(5, 7, 'Blue');

console.log(myRectangle.getArea()); // 35
console.log(myRectangle.fill()); // This Rectangle is Blue
{% endhighlight %}
