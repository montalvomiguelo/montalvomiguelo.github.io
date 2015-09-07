---
layout: post
title:  "Introducción a SASS"
date:   2014-10-01 20:30
categories: frontend
---
Sass (Semantically Awesome Stylesheets) es un pre-procesador de CSS inventado en 2006 por @hcatlin. Funciona como un compilador, tu como desarrollador escribes Sass y te convierte a CSS que puede entender cualquier navegador.

Sass puede correr en cualquier sistema operativo Mac, Linux ó Windows y esta escrito en Ruby por lo cual primero deberás tener instaladas las gemas. Si tienes una Mac, Ruby ya viene instalado por defecto y lo único que tienes que hacer es abrir tu terminal y ejecutar lo siguiente:

{% highlight bash %}
$ gem install sass
{% endhighlight %}

Para saber que versión de Sass tenemos instalada ejecutamos el siguiente comando:

{% highlight bash %}
$ sass --version
{% endhighlight %}

Una vez que lo hemos instalado vamos a crear nuestro primer archivo test.scss con el siguiente código para compilarlo a .css

{% highlight scss %}
body {
  background: white;
  margin: 0 auto;
  width: 960px;
}
{% endhighlight %}

Ejecutamos en la terminal:

{% highlight bash %}
$ sass test.scss
{% endhighlight %}

Y nos arrojara el siguiente css:

{% highlight css %}
body {
  background: white;
  margin: 0 auto;
  width: 960px;
}
{% endhighlight %}

Seguramente queremos seguir editando nuestro archivo fuente test.scss y que en forma automática compile a .css en este caso ejecutaremos lo siguiente:

{% highlight bash %}
$ sass --watch test.scss
{% endhighlight %}

Esto te generará un archivo test.css y cada vez que realices un cambio en tu archivo automáticamente se actualizará tu salida test.css.
