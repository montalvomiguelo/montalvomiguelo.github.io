---
layout: post
title:  "Variables en SASS"
date:   2015-09-04 18:04:11
categories: frontend
---
Una variable es un nombre que representa un valor, en Sass las variables comienzan con el signo de dollar:

{% highlight sass %}
$primary-color: #333;
{% endhighlight %}

Esto es muy útil para crear un código que sea fácil de leer y mantener. En el siguiente fragmento de código vemos un buen uso de las variables y como nos evitan reescribir el mismo color hexadecimal una y otra vez. Además si queremos cambiar el color en un futuro, lo haremos solo modificando la variable y toda nuestra hoja de estilos estará actualizada.

{% highlight sass %}
$primaryColor: #34495e;

body {
  background: $primaryColor;
}

article {
  background: white;
  color: $primaryColor;
  &:hover {
    color: darken($primaryColor, 15%);
  }
}
{% endhighlight %}
