---
title:  "Trabajando con el el constructor Object"
categories: blog
---
Todo en JavaScript es un objeto, heredan propiedades y métodos del
constructor `Object`.

## prototype
`prototype` es una __propiedad__ que tiene el constructor `Object` que no es mas que una cadena de
funcionalidades entre las cuales podemos encontrar métodos como: `hasOwnProperty`,
`propertyIsEnumerable`, `toString`, etc.

Es considerada una __propiedad viva__ de tal modo que si nosotros
agregamos propiedades o funcionalidades en esta propiedad, automáticamente serán
heredadas a todos los demás objetos de JavaScript pues todos heredan del
`prototype` de `Object`.

{% highlight js %}
var obj = {};

Object.prototype.sayHello = function() {
  console.log('Hello');
}

obj.sayHello(); // Hello

var arr = [];
var num = 1;

arr.sayHello(); // Hello
num.sayHello(); // Hello
{% endhighlight %}

No es una práctica muy recomendada pero es útil cuando queremos agregar
métodos que no están soportados en navegadores viejos. Ejemplo [es5-shim][es5-shim]

[es5-shim]: https://github.com/es-shims/es5-shim/blob/master/es5-shim.js

## keys
Esté método de `Object` devuelve un arreglo con las propiedades enumerables de un objeto dado.

{% highlight js %}
var person = {
  nombre: 'Miguel',
  edad: 23
}

console.log( Object.keys(person) ); // ["nombre", "edad"]
{% endhighlight %}

## freeze
Este método retorna un objeto congelado, esto quiere desir que no es
posible agregar/eliminar ni editar propiedades.

{% highlight js %}
var person = {
  nombre: 'Miguel',
  edad: '23'
}

Object.freeze(person);

person.nombre = 'Updated'; // Uncaught TypeError: Cannot assign to read only property 'nombre' of object '#<Object>'
{% endhighlight %}

Una vez que el objeto ha sido congelado, no se puede descongelar, __no existe__ `unFreeze`.
Si necesitamos modificar valores, podemos crear una copia del objeto
con ayuda del método `keys` de la siguiente manera:

{% highlight js %}
var descongelado = {};

Object.keys(person).forEach(function(key) {
  descongelado[key] = person[key];
});

descongelado.edad = 24;

console.log(descongelado); // Object {nombre: "Miguel", edad: 24}
{% endhighlight %}

## seal
Este método es similar a `freeze`, no podemos agregar ni eliminar
propiedades, pero si podemos modificar las existentes.

{% highlight js %}

Object.seal(descongelado);

descongelado.edad = 25;

console.log(descongelado); // Object {nombre: "Miguel", edad: 25}

descongelado.pelo = 'chino'; // Uncaught TypeError: Can't add property pelo, object is not extensible
{% endhighlight %}






