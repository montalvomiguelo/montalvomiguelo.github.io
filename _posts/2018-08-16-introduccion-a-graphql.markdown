---
title:  "Introducción a GraphQL"
categories: blog
tags: JavaScript
---
GraphQL se divide en dos partes, el lenguaje de consultas  que es la
sintaxis utilizada para escribir consultas y un framework en el lado del
servidor que se encarga de procesar las consultas.

En GraphQL se utiliza un sistema de tipado de datos que sirve para
describir la forma en que se pueden recibir/enviar los datos y esto lo
hace muy intuitivo pues con solo leer el *schema* donde están definidos
los tipos de objetos que tiene el API, se puede intuir como es su uso.

Ejemplo el tipo de objeto `Product`, se asemeja bastante al formato JSON y
sin saber aun nada de GraphQL se puede ver que un `Product` va a tener
una propiedad `id`, `title` y `price`.

{% highlight text %}
{
  type Product {
    id: ID!
    title: String!
    price: Float!
  }
}
{% endhighlight %}

Con GraphQL es muy sencillo solicitar datos anidados en una sola
consulta. Por ejemplo un `Product` puede tener una propiedad llamada `collection`
de tipo `Collection` con sus respectivas propiedades.

{% highlight diff %}
{
  type Product {
    id: ID!
    title: String!
    price: Float!
+   collection: Collection
  }

+ type Collection {
+   id: ID!
+   title: String!
+ }
}
{% endhighlight %}

## GraphQL vs REST

REST es un estilo para escribir APIs donde se exponen funcionalidades
para manipular *recursos* sobre urls con los verbos de http `GET`, `POST`,
`PUT`, `DELETE`.

Recurso `Product`:

* GET shopapi.com/products *(Listado de todos los productos)*
* GET shopapi.com/products/:id *(Un producto por ID)*

Recurso `Collection`:

* GET shopapi.com/collections *(Listado de todas las colecciones)*
* GET shopapi.com/collections/:id *(Una colección por ID)*

Petición de un producto por ID en la url shopapi.com/products/001.

{% highlight json %}
{
  "id": "001",
  "title": "iPad",
  "price": "1000",
  "collection": "100"
}
{% endhighlight %}

En la respuesta de este producto la propiedad `collection` es un ID,
entonces si se desea saber más información de esa colección se tiene
que hacer otra petición:

shopapi.com/collections/100

{% highlight json %}
{
  "id": "100",
  "title": "Apple"
}
{% endhighlight %}

Tradicionalmente así esa es la manera en la que funcionan las APIs allá
afuera.

Ahora vamos a ver como sería esa consulta con GraphQL

```text
{
  query {
    product(id: "product_1") {
      title
      price
      collection {
        title
      }
    }
  }
}
```

Esta consulta se hace únicamente en una petición al servidor y la
data de la propiedad `collection` ya viene incluida. Además es posible
especificar la forma de la respuesta escribiendo únicamente las
propiedades que se necesitan de ella.

## Terminología de GraphQL

* Queries .- Es lo que escribimos para hacer una consulta a un servidor
  GraphQL donde básicamente seleccionamos propiedades o fields de objetos.
* Fields .- Son propiedades que componen la forma de un tipo de objeto de
  consulta. Estos se incluyen o excluyen de la consulta para definir como
queremos que sea la respuesta.
* Types .- Son un conjunto de fields que componen un tipo de objeto
  de consulta.

```text
type Product {
  id: ID!
  title: String!
  price: Float!
}
```

## Apollo Launchpad

Tengo un Pad preparado para empezar a escribir nuestras primeras
consultas GraphQL construido con Apollo Launchpad que es un servicio tipo
CodePen pero para GraphQL.

[https://launchpad.graphql.com/lkqqlww31q][pad]

* En el panel izquierdo se encuentra el lado del servidor.
* En el panel central es donde se escriben las consultas.
* En el panel derecho es se muestran los resultados.

En el panel izquierdo veremos un tipo de objeto llamado `Product` que
tiene los fields `id`, `title`, `price` y `collection`.

Para escribir una consulta comenzamos con la palabra `query` y
abrimos llaves como si fuera un JSON. Después escribimos el *endpoint*
`products` el cual nos regresará un listado de objetos de tipo `Product` y por ultimo
definimos la forma de la respuesta agregando únicamente las propiedades
o fields que nos interesan de `Product` por ejemplo `title` y `price`.

![Primer consulta graphql]({{ "/assets/fetch-products.gif" }})

## Anatomia de un Query

Un query o consulta de GraphQL se compone de tres partes:

### Declaración de la consulta
Comienza con la declaración `query` que le indica a GraphQL que únicamente queremos obtener
información seguido de la apertura de llaves.

{% highlight diff %}
+ query {
+ }
{% endhighlight %}

### Endpoint
Llamada al *endpoint* que resolverá la consulta.

{% highlight diff %}
query {
+ products {
+ }
}
{% endhighlight %}

### Fields
Definir la forma de la respuesta incluyendo o excluyendo los fields del
tipo de objeto que regresa la consulta.

{% highlight diff %}
query {
  products {
+   title
+   price
  }
}
{% endhighlight %}

## Scalar types

Cada objeto de consulta tiene sus propios fields definidos con un
tipo de dato.

Con GraphQL por defecto se pueden utilizar los siguientes tipos
de datos escalares o simples.

* `ID`
* `Int`
* `Float`
* `String`
* `Boolean`

{% highlight text %}
{
  type Product {
    id: ID!
    title: String!
    price: Float!
  }
}
{% endhighlight %}

## Object type field

Un objeto de consulta puede tener fields de tipo `object` que a su vez se
compone de otros fields escalares o de tipo `object`.

{% highlight diff %}
{
  type Product {
    id: ID!
    title: String!
    price: Float!
+   collection: Collection
  }

+ type Collection {
+   id: ID!
+   title: String!
+ }
}
{% endhighlight %}

Cuando se tiene un tipo de dato `object` se debe especificar en la
consulta que fields de ese `object` se necesitan en la respuesta.

![Fetch with object field]({{ "/assets/fetch-with-object-field.gif" }})

## Mutation

Es una consulta especial en GraphQL para cambiar datos en el servidor.
La anatomía de una consulta *mutation* es la siguiente:

### Declaración de la consulta

Con la palabra `mutation` que le indica a  a GraphQL que vamos a cambiar algo, seguido
de la apertura de llaves.

{% highlight diff %}
+ mutation {
+ }
{% endhighlight %}

### Endpoint

Llamado al *endpoint* que resolverá la consulta y los argumentos que
necesita para procesar la consulta. En los argumentos es necesario
especificar su nombre en formato de *llave: valor*.

Para saber que argumentos necesita podemos revisar la definición de la *mutation* en el schema, además
podemos ver de que tipo es la respuesta.

*(Todos los fields con el bang `!` son requeridos)*.

{% highlight text %}
type Mutation {
  productCreate(
    title: String!
    price: Float!
  ): Product
}
{% endhighlight %}

{% highlight diff %}
mutation {
+ productCreate(
+   title: "MacBook Pro"
+   price: 1299.00
+ ) {
+ }
}
{% endhighlight %}

### Fields

Define la forma de la respuesta que regresa la consulta.

{% highlight diff %}
mutation {
  productCreate(
    title: "MacBook Pro"
    price: 1299.00
  ) {
+   id
+   title
  }
}
{% endhighlight %}

![Primer mutation]({{ "/assets/mutation-scalar.gif" }})

## Mutaciones con objetos de entrada

En una *mutation* es posible utilizar fields o propiedades de tipo `input object` que tienen sus
propios fields escalares o de tipo `object`.

{% highlight diff %}
type Mutation {
  productVariantCreate(
    productId: ID!
+   variant: VariantInput
  ): Product
}

+ input VariantInput {
+   title: String!
+ }
{% endhighlight %}

Para escribir esta consulta nos podemos apoyar de variables de consulta
de GraphQL. Estas variables de consulta se definen en formato JSON en el
panel de *Query Variables*.

![Query Variable]({{ "/assets/variant-to-add.gif" }})

Tenemos un *endpoint* `productVariantCreate` donde de acuerdo a su definición en el schema requiere que le pasemos como
argumentos el `productId` y el `variant` que es un objeto de entrada.

También vamos a utilizar variable de consulta `variantToAdd` como argumento en
la *mutation* donde se debe especificar su tipo.

{% highlight diff %}
+ mutation($variantToAdd: VariantInput!) {
    productVariantCreate(
      productId: "product_2"
+     variant: $variantToAdd
    ) {
      title
      variants {
        title
      }
    }
  }
{% endhighlight %}

*(Las variables de consulta en GraphQL se escriben como si
fueran variables de PHP con el signo dólar al inicio)*.

![Object input]({{ "/assets/object-input.gif" }})

## Operaciones Nombradas

En GraphQL podemos dar nombre a las operaciones que realicemos. A esto se
le conoce como *Aliases*.

## Aliases

Esto es de mucha ayuda cuando queremos realizar multiples consultas al
mismo *endpoint* con distintos argumentos en una sola petición.

Para agregar un *alias* lo escribimos antes de el *endpoint* seguido de
dos puntos `:`.

{% highlight diff %}
query {
+ iPad: product(id: "product_1") {
    title
  }
}
{% endhighlight %}

![Aliases]({{ "/assets/aliases.gif" }})

Si no se usan *aliases* y se intenta hacer un llamado al mismo *endpoint* mas de una vez, se
ocasionará una coalición en el nombre de las llamadas sobre el mismo
*endpoint*.

Un *endpoint* solo puede regresar un resultado a la vez.
Entonces GraphQL nos va a sugerir que hagamos uso de *aliases* para
renombrar el resultado de un *endpoint* y evitar la coalición de nombres.

## Fragments

Se utilizan para agrupar *fields* sobre un tipo de objeto en específico
y poder reutilizarlo en nuestras consultas para evitar la repetición.

Un *fragment* lo incluimos en una consulta de la siguiente manera:

{% highlight diff %}
query {
  iPad: product(id: "product_1") {
+   ...productDetails
  }
}
{% endhighlight %}

Se define con la palabra `fragment`, un nombre y el tipo de objeto al
que pertenecen estas propiedades.

{% highlight diff %}
+ fragment productDetails on Product {
+   id
+   title
+   price
+ }
{% endhighlight %}

![Fragments]({{ "/assets/fragments.gif" }})

[pad]: https://launchpad.graphql.com/lkqqlww31q
