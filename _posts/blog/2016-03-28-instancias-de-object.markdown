---
title:  "Instancias de Object"
categories: blog
---
Cada vez que creamos un nuevo objeto en JavaScript este hereda en
automático las propiedades y métodos del constructor `Object`.

Ejemplo la propiedad `constructor`, está disponible en cualquier
instancia de Object:

{% highlight js %}
var obj = {};

console.log(obj.constructor); // Object() { [native code] }
{% endhighlight %}

## hasOwnProperty
Este método también está disponible en todos los objetos de JavaScript y
permite evaluar si el un objeto contiene una propiedad y entonces
retorna `true`. Si es una propiedad que es heredada de la cadena de
`prototype` entonces devuelve `false`.

{% highlight js %}
console.log(obj.hasOwnProperty('constructor')); // false

obj.color = 'blue';

console.log(obj.hasOwnProperty('color')); // true
{% endhighlight %}

## propertyIsEnumerable
Evalúa si una propiedad dentro de un objeto puede ser iterada con
la sentencia `for ..in`. Las propiedades propias del objeto son
enumerables por defecto. Las propiedades que son heredadas de la cadena
`prototype` pueden ser o no enumerables al momento de ser definidas.

{% highlight js %}
console.log(obj.propertyIsEnumerable('color')); // true

console.log(obj.propertyIsEnumerable('constructor')); // false
{% endhighlight %}

## toString
Todos los objetos heredan este método y sirve para retornar una
representación en cadena de texto del objeto.

{% highlight js %}
console.log(obj.toString()); // [object Object]

console.log(['a', 'b', 'c'].toString()); // a,b,c
{% endhighlight %}

Como se puede observar cada constructor implementa su propia versión del método `toString`.
Si quisiéramos utilizar el método `troString` del constructor `Object`
haríamos lo siguiente:

{% highlight js %}
console.log(Object.prototype.toString.call(['a', 'b'])); // [object Array]
{% endhighlight %}
