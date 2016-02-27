---
title:  "Variables en Sass"
categories: blog
---
En Sass las variables comienzan con el signo de dollar. Nos
van a permitir definir valores en un solo lugar y reutilizarlos a lo largo de nuestro proyecto.

{% highlight scss %}
$primary-color: #1abc9c;
{% endhighlight %}

En el siguiente fragmento de código vemos un ejemplo de uso de las variables y como nos evitan reescribir el mismo color hexadecimal una y otra vez. Además si queremos cambiar el color en un futuro, lo haremos solo modificando la variable y toda nuestra hoja de estilos estará actualizada sin hacer cosas como Search & Replace.

{% highlight scss %}
$primary-color: #1abc9c;

body {
  color: $primary-color;
}

form {
  input {
    background: initial;
    border: 1px solid $primary-color;
    color: $primary-color;
    padding: .5em;
  }
}
{% endhighlight %}
