---
title:  "Entendiendo las Promesas de JavaScript"
categories: blog
tags: Javascript
---
## Callback hell
Es muy común que al estar escribiendo JavaScript hagamos uso de
callbacks (funciones que se ejecutan después de que algo pasó). Por
ejemplo cuando queremos hacer una petición AJAX:

{% highlight javascript %}
getShop('affliction', (err, shop) => {
  if (err) return console.error(err);
  activateSession(shop, (err, status) => {
    if (err) return console.error(err);
    doSomething(status);
  });
});
{% endhighlight %}

En este ejemplo sencillo se están utilizando dos callbacks, uno para la
función `getShop` y otro para la función `activateSession` que se van
anidando uno dentro de otro. Y podrían ser más, a esto se le conoce como
*Pyramid of Doom* ó *Callback Hell* y esto no es fácil de leer ni
entender.

## Promises
Promises o Promesas es una nueva funcionalidad de JavaScript en ES6 y
es soportada actualmente por todos los navegadores modernos. Nos permite
manipular operaciones asíncronas de una manera más sencilla.

Si `getShop` y `activateSession` fueran Promises, el ejemplo anterior
sería así:

{% highlight javascript %}
getShop('affliction')
  .then(activateSession)
  .then(doSomething)
  .catch(err => console.error(err));
{% endhighlight %}

Una Promesa es un objeto que representa un valor eventual para una
operación asíncrona, por ejemplo una petición AJAX.

Una Promesa puede tener lo siguientes estados:

* *Pending:* es el estado inicial en espera de que la operación asíncrona termine.
* *Fulfilled / Resolved:* cuando la operación asíncrona terminó.
* *Rejected:* cuando hubo algún error durante la operación asíncrona.

### .then
Para poder hacer algo en esos estados de la Promesa se utiliza el método
`then` que recibe como primer argumento la función que manipula el estado *Resolved*.

Como segundo argumento opcionalmente puede recibir otra función para manipular el
estado *Rejected*. Pero no es muy común usarlo así. Normalmente uno para
manipular el estado *Rejected* usa el método `.catch`.

```
Promise.prototype.then(onFulfilled, onRejected)
```

{% highlight diff %}
getShop('affliction')
+  .then(shop => console.dir(shop));
{% endhighlight %}

El método `then` retorna una Promesa, entonces esto nos permite
encadenar más Promesas.

{% highlight diff %}
getShop('affliction')
+  .then(activateSession)
+  .then(doSomething);
{% endhighlight %}

### .catch
No es necesario manipular el estado *Reject* de una Promesa, por defecto la
promesa fallará silenciosamente pero no afectará a la ejecución del
resto de la aplicación.

Si queremos manipular el estado *Reject* podemos usar el método `catch`
que recibe la función que manipula este estado.

```
Promise.prototype.catch(onRejected);
```

{% highlight diff %}
getShop('affliction')
  .then(activateSession)
  .then(doSomething)
+ .catch(err => console.log(err));
{% endhighlight %}

## Cómo escribir una Promesa

Una Promesa es una instancia del constructor Promise que recibe una
función con dos argumentos:

* Una función para manipular el estado *Resolved*
* Una función para manipular el estado *Reject*

Dentro de la promesa vamos a llamar la operación asíncrona y utilizar las
funciones para el estado *Resolved* y *Reject*.

{% highlight javascript %}
const getShop = function(shopName) {
  return new Promise((resolve, reject) => {
    // Esta es una operación asíncrona
    setTimeout(() => {
      const shop = JSON.stringify({
        id: 1,
        shop: `${shopName}.myshopify.com`,
        token: 't0k3n'
      });

      if (!shopName) {
        return reject('Invalid name');
      }

      resolve(shop);
    }, 2300);
  });
};

const activateSession = function(shop) {
  return new Promise((resolve) => {
    // Esta es otra operación asíncrona
    setTimeout(() => {
      const status = shop ? true : false;

      if (!shop) {
        return reject('Invalid shop');
      }

      resolve(status);
    }, 2300);
  });
};
{% endhighlight %}
