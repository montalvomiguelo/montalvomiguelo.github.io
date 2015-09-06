---
layout: post
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

##Selectores anidados

Como ya habrás notado estamos repitiendo código el cual es más difícil de leer  y mantener. Sass nor permite arreglar eso con selectores anidados veamos como funciona. Creamos un archivo llamado main.scss y escribimos el siguiente código:

{% highlight sass %}
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

La sintaxis de Sass es igual que la de CSS tradicional pero mira como utilizamos las llaves para definir los elementos que queremos afectar. En este caso estamos afectando a todas las etiquetas a, h1 y p que como puedes observar estan dentro de las llaves de .blog-entry. Entonces al compilar este archivo tendremos el siguiente resultado y Sass habrá hecho todo el trabajo de repetir código por nosotros.

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

##Parent Selector

Esto nos permite insertar los selectores que son padres en frente de nuestro selector hijo. Por ejemplo, tenemos la librería de modernizr y queremos dar estilo a las etiquetas p dentro de .blog-entry cuando tengamos html.csscolumns. entonces hacemos lo siguiente:

{% highlight sass %}
body
{
  background: #eee;
}

.blog-entry
{
  a
  {
    color: red;
    &:hover
    {
      color: pink;
    }
  }

  h1
  {
    color: blue;
    font-size: 20px;
  }

  p
  {
    font-size: 12px;

    html.csscolumns &
    {
      font-weight: bold;
    }

  }
}
{% endhighlight %}
