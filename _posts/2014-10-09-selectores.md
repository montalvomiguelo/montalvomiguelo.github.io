---
title:  "Selectores"
date:   2014-10-09 22:04:11
categories: frontend
---
Escribir CSS con Sass es mucho más fácil que hacerlo de la manera tradicional. Supongamos que tenemos el siguiente markup:


{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title></title>
  <link rel="stylesheet" href="main.css">
  <script src="http://modernizr.com/downloads/modernizr-latest.js"></script>
</head>
<body>
  <div class="blog-entry">
    <h1>My Blog post</h1>
    <p>Text <a href="#">Link</a></p>
  </div>
</body>
</html>
{% endhighlight %}

Y queremos dar estilo a las etiquetas `a`, `h1`, `p` que están dentro de `.blog-entry`. De la manera tradicional tendríamos que escribir lo siguiente.

{% highlight css %}
body {
  background: #eee;
}

.blog-entry a {
  color: red;
}

.blog-entry h1 {
  color: blue;
  font-size: 20px;
}

.blog-entry p {
  font-size: 12px;
}
{% endhighlight %}

## Selectores anidados

Como ya habrás notado estamos repitiendo código el cual es más difícil de leer  y mantener. Sass nos permite arreglar eso con selectores anidados, veamos como funciona. Creamos un archivo llamado main.scss y escribimos el siguiente código:

{% highlight scss %}
body
{
  background: #eee;
}

.blog-entry
{
  a
  {
    color: red;
  }

  h1
  {
    color: blue;
    font-size: 20px;
  }

  p
  {
    font-size: 12px;
  }
}
{% endhighlight %}

La sintaxis de Sass es igual que la de CSS tradicional pero mira como utilizamos las llaves
para definir los elementos que queremos afectar. En este caso estamos afectando a todas
las etiquetas `a`, `h1` y `p` que como puedes observar están dentro de las
llaves de `.blog-entry`. Entonces al compilar este archivo tendremos el siguiente
resultado y Sass habrá hecho todo el trabajo de repetir código por nosotros.

{% highlight css %}
body {
  background: #eee; }
.blog-entry a {
  color: red; }
.blog-entry h1 {
  color: blue;
  font-size: 20px; }
.blog-entry p {
  font-size: 12px; }
{% endhighlight %}

## Parent Selector

Hacemos referencia al selector padre utilizando el _ampersand_ `&`. Esto es una herramienta muy poderosa si lo utilizamos
correctamente, te permite escribir menos código, tener tus estilos muy
legibles y fáciles de mantener.

Como ejemplo vamos a dar estilos a una navegación escrita con la
convención de nombres BEM.

{% highlight html %}
<ul class="menu">
  <li class="menu__item"><a href="#">Link 1</a></li>
  <li class="menu__item"><a href="#">Link 2</a></li>
  <li class="menu__item"><a href="#">Link 3</a></li>
</ul>
{% endhighlight %}

{% highlight scss %}
.menu {
  margin: 0;
  padding: 0;
  &__item {
    display: inline-block;
    list-style: none;
  }
}
{% endhighlight %}

Mira como el parent selector `&` hace referencia en este caso a la clase
`.menu` y no la tuvimos que reescribir de nuevo:

{% highlight scss %}
.menu {
  margin: 0;
  padding: 0;
}
.menu__item {
  display: inline-block;
  list-style: none;
}
{% endhighlight %}
