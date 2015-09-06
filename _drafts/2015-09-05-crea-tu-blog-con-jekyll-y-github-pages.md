---
layout: post
title:  "Crea tu blog con Jekyll y Github Pages"
categories: Jekyll
author: montalvomiguelo
---
Con Jekyll puedes crear tu propio blog personal, un portafolio o cualquier tipo
de sitio que tenga una serie de entradas de post sin tener que depender
de una base de datos y un CMS.

Jekyll se encargará de generar tu sitio estático desde tu computadora y estarás
listo para poder subirlo a cualquier servidor web.

##¿Qué es Jekyll?
Jekyll es un generador de sitios estáticos escrito en Ruby. Un generador
de sitios estáticos toma una serie de archivos de texto (escritos en Markdown) y
templates que son procesados para finalmente
obtener páginas de HTML listas para publicar en cualquier servidor.

Jekyll está pensado para crear blogs estáticos. Podemos publicar y mantener nuestro blog simplemente gestionando
un folder de archivos de texto.

Otros generadores de sitios estáticos populares son:

* [Harp][harp-site]
* [Middleman][middleman-site]
* [Octopress][octopress-site]

##Ventajas al utilizar un generador de sitios estáticos como Jekyll

* Velocidad: Estamos sirviendo páginas estáticas, no necesitamos hablar
  con una base de datos para pedirle la información en cada request ni
renderear datos en el servidor.

* Seguridad: No hay una base de datos o contenido dinámico que pueda
  ser hackeado.

* Menos mantenimiento: Al no tener base de datos no tenemos que
  preocuparnos por configurarla o mantenerla.

* Hosting gratuito: Podemos alojar nuestro sitio de manera gratuita en
  Github Pages.

###¿Quiénes están utilizando Jekyll?

* [Portal de datos.gob.mx][datos-gob-mx]
* [Jekyll][jekyll]
* [CSS Wizardry][csswizardry]
* [HealthCare.gov][healthcare]
* [Github Pages][github-pages] de tras de escenas utiliza Jekyll para generar los sitios
  estáticos.

##Instalación de Jekyll

Abrimos la terminal y escribimos el siguiente comando para instalar Jekyll (en la Mac). Si tu tienes otro
sistema operativo te recomiendo revisar la [documentación de instalación][jekyll-installation].

{% highlight bash %}
$ sudo gem install jekyll
{% endhighlight %}

##Crear un nuevo proyecto con Jekyll

Escribimos el siguiente comando en la terminal para crear un nuevo proyecto con
Jekyll:

{% highlight bash %}
$ jekyll new myblog
{% endhighlight %}

Esto creará un directorio llamado `myblog/` donde se encuentran todos
los archivos necesarios para crear nuestro blog.

{% highlight bash %}
▸ _includes/
▸ _layouts/
▸ _posts/
▸ _sass/
▸ css/
  _config.yml
  about.md
  feed.xml
  index.html
{% endhighlight %}

`_config.yml` probablemente es uno de los más importantes, en
este archivo configuramos a Jekyll para indicarle como va a construir nuestro sitio.

El directorio `_includes/` contiene fragmentos de código reutilizables para
incluir en nuestros templates y posts.

El directorio `_layouts/` contiene los templates necesarios para mostrar
nuestras páginas y posts.

En `_posts/` es donde organizaremos nuestras entradas del
blog en archivos de texto con formato markdown.

[Markdown][markdown] es una manera simple de leer/escribir en texto plano documentos web
que después se convertirán en HTML.

Jekyll tiene soporte para SASS y en el directorio `_sass/` se encontrarán todos
nuestros partials. El archivo principal de sass se encuentra en `css/`
y se encarga de llamar a los partials y es compilado por Jekyll
para generar el código css.
Esto es completamente opcional, si alguien quiere trabajar con css puro
puede utilizar solo el folder `css/` y  escribir css de manera
tradicional.

`index.html` es la portada de nuestro sitio. Pero si tratamos de abrir
ese archivo aun no veremos nada y esto es por que no le hemos dicho a
Jekyll que _genere nuestro sitio estático_.

###Generar el sitio estático con Jekyll

Jekyll tienen un servidor de desarrollo para que podamos mirar nuestro
blog en nuestro equipo local. Para que Jekyll genere y sirva nuestro
sitio ejecutamos el siguiente comando en la terminal. (Debes estar
situado dentro del directorio de tu proyecto generado con Jekyll).

{% highlight bash %}
$ jekyll serve
{% endhighlight %}

Al hacer esto Jekyll generará nuestro sitio estático en el directorio
`_site/` y nos levantará un servidor local en la url `http://127.0.0.1:4000/` observando los cambios que hagamos
en nuestro blog.

Jekyll tiene un tema por defecto bastante bueno con un post y una página
de ejemplo que podemos utilizar para aprender como crear más contenido
en nuestro blog.

Observa un poco la estructura de archivos de tu proyecto Jekyll, notarás
que algunos comienzan con underscore `_`. Estos archivos/directorios son leídos por
Jekyll para generar el sitio estático en el directorio `_site/`, pero no
serán copiados en el sitio final. Todo lo que no comienza con `_` si
es copiado directamente al sitio final como por ejemplo el directorio
`css/`

Es importante que tengamos esto en cuenta, por ejemplo si queremos incluir un
directorio de assets para guardar imágenes que mas tarde incluiremos en
nuestros posts, creamos un directorio llamado `assets/` en el root del
proyecto.

[markdown]: http://daringfireball.net/projects/markdown/
[jekyll-installation]: http://jekyllrb.com/docs/installation/
[harp-site]: http://harpjs.com/
[middleman-site]: https://middlemanapp.com/
[octopress-site]: http://octopress.org/
[datos-gob-mx]: http://datos.gob.mx/
[jekyll]: http://jekyllrb.com/
[csswizardry]: http://csswizardry.com/
[healthcare]: https://www.healthcare.gov/
[github-pages]: https://pages.github.com/
[jekyll-configuration]: http://jekyllrb.com/docs/configuration/
[yaml-site]: http://yaml.org/
[permalinks-doc]: http://jekyllrb.com/docs/permalinks/#built-in-permalink-styles
[frontmatter-doc]: http://jekyllrb.com/docs/frontmatter/

##[Configuración de Jekyll][jekyll-configuration]

Las configuraciones de nuestro proyecto se establecen en el archivo
`_config.yml`. [YAML][yaml-site] es un formato especial para guardar configuraciones
y settings con una sintaxis muy fácil de leer.

{% highlight yaml %}
# Site settings
title: Your awesome title
email: your-email@domain.com
description: > # this means to ignore newlines until "baseurl:"
  Write an awesome description for your new site here. You can edit this
  line in _config.yml. It will appear in your document head meta (for
  Google search results) and in your feed.xml site description.
baseurl: "" # the subpath of your site, e.g. /blog/
url: "http://yourdomain.com" # the base hostname & protocol for your site
twitter_username: jekyllrb
github_username:  jekyll

# Build settings
markdown: kramdown
{% endhighlight %}

Dentro de este archivo se encuentran las variables que ya están
establecidas por Jekyll para configurar nuestro sitio. Cada vez que
hagamos cambios en `_config.yml` es necesario que detengamos el servidor
`Ctrl+c` y lo levantemos de nuevo con:

{% highlight bash %}
$ jekyll serve
{% endhighlight %}

###Configuración de permalinks
Por defecto los permalinks de Jekyll son algo como:

```
http://localhost:4000/jekyll/update/2015/09/04/welcome-to-jekyll.html
```

Pero podemos cambiar el estilo de los permalinks con la variable
`permalink`. Jekyll tiene varios estilos para los permalinks, lee la
[documentación sobre este tema][permalinks-doc], a mi me gusta estilo `pretty`
entonces agrego lo siguiente al final de `_config.yml` y asegurate de
reiniciar el servidor cada vez que toques este archivo.

{% highlight yaml %}
permalink: pretty
{% endhighlight %}

##[YAML Front Matter][frontmatter-doc]

Front Matter es una de las características mas bonitas que tiene Jekyll.
El YAML Front Matter se usa para guardar informacion acerca de nuestras páginas tales como:

* `layout` Template que será utilizado para mostrar esta entrada
* `title` El título de la entrada
* `date` Fecha de publicación.
* `categories` Categoria de la entrada
* `author` Autor de la entrada

Incluso puede almacenar valores personalizados como una imágen destacada,
un color de fondo y en general cualquier valor que necesite tener nuestro
contenido.

Si abrimos el post que viene de ejemplo con Jekyll, veremos
al inicio del archivo el YAML front matter.

{% highlight text %}
---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-09-04 22:04:11
categories: jekyll update
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
{% endhighlight %}

El YAML Front Matter siempre debe estár al inicio de el archivo, y su
contenido se encuentra dento del bloque que abre y cierra con 3 guiones:

{% highlight text %}
---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-09-04 22:04:11
categories: jekyll update
---
{% endhighlight %}

Cada archivo que tenga el YAML Front Matter será rendereado de manera
especial por Jekyll.

##Crea tu primer post

Para que las entradas del blog sean ordenadas corretamente por
Jekyll, es necesario nombrar los archivos con la siguiente convensión:

```year-month-day-post-name.md```
