---
title:  "Funciones y Closures"
categories: blog
tags: Javascript
---
Un closure es simplemente un comportamiento de las funciones en
JavaScript. Sucede cuando una __función padre__ retorna una __función hija__, al hacer esto la __función hija__ recordará el scope donde fué definida:


{% highlight js %}
var ShoppingCart = function() {
  var total = 0;

  return function addProduct(price) {
    return total+= price;
  };

};

var myShoppingCart = new ShoppingCart();

console.log(myShoppingCart(7)); // 7
console.log(myShoppingCart(5)); // 12
{% endhighlight %}
