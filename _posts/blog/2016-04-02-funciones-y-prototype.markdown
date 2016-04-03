---
title:  "Funciones y Prototype"
categories: blog
---
Para crear objetos personalizados podemos crear nuestros propios
constructores a través de funciones con métodos y propiedades
personalizados que colocamos en el `prototype` de nuestro constructor.
Ejemplo _constructor de Persona_:

{% highlight js %}
var Person = function(name) {
  this.name = name;
};

Person.prototype.greet = function() {
  console.log('Hola, soy ' + this.name);
};

Person.prototype.setAge = function(age) {
  this.age = age;
};

var pepito = new Person('Pepito');

pepito.greet(); // Hola, soy Pepito

console.log(pepito.constructor.prototype); // Object {}

{% endhighlight %}

## Operador new
Para utilizar nuestro constructor `Person` se utiliza el operador `new`
con el cual creamos una instancia de `Person` que hereda todos los
métodos y propiedades definidos en el `prototype` del constructor
`Person`.

## Object.create
Otra opción para crear instancias de nuestro constructor `Person` es a
través del método `create` del constructor de `Object`:

{% highlight js %}
var juanito = Object.create(Person.prototype, {
  name: {
    writable: true,
    value: 'Juanito'
  }
});

juanito.greet(); // Hola, soy Juanito
{% endhighlight %}
La ventaja de este método es que podemos configurar cómo se comportan
las propiedades de nuestra instancia, `writable`, `configurable`, `enumerable`, etc.

## Object.defineProperty
Este método nos permite definir una nueva propiedad sobre un objeto ó
modificar una existente:

{% highlight js %}
Object.defineProperty(juanito, 'job', {
  value: 'Artist',
  writable: true
});

console.log(juanito); // Person {name: "Juanito", job: "Artist"}
{% endhighlight %}
