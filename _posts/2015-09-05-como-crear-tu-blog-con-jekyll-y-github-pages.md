---
layout: post
title:  "Cómo crear tu Blog con Jekyll y Github Pages"
categories: Jekyll
---
Con Jekyll puedes crear tu propio blog personal, un portafolio o cualquier tipo
de sitio que tenga una serie de entradas de post sin que dependas de una
base de datos ni un CMS.

He migrado mi blog personal de WordPress a Jekyll y en este post quiero
compartir contigo cómo fue este proceso desde la instalación de Jekyll, configuración, su
estructura de archivos, cómo funciona y como publicarlo de manera
gratuita en Github Pages con tu dominio personal.

## ¿Qué es Jekyll?
> Jekyll is a simple, blog-aware, static site generator.

Jekyll es un generador de sitios estáticos escrito en Ruby. Un generador
de sitios estáticos toma una serie de archivos de texto (escritos en Markdown) y
templates que son procesados para finalmente
obtener páginas de HTML listas para publicar en cualquier servidor.

Jekyll está pensado para crear blogs estáticos. Podemos publicar y mantener nuestro blog simplemente gestionando
un directorio de archivos de texto.

Con Jekyll puedes sustituir a WordPress, Joomla o cualquier CMS, cuenta con urls
amigables, categorías, páginas, posts, templates, etc.

## Ventajas al utilizar un generador de sitios estáticos como Jekyll

* Velocidad: Estamos sirviendo páginas estáticas, no necesitamos hablar
  con una base de datos para pedirle la información en cada request ni
renderear datos en el servidor.

* Seguridad: No hay una base de datos o contenido dinámico que pueda
  ser hackeado.

* Menos mantenimiento: Al no tener base de datos no tenemos que
  preocuparnos por configurarla o mantenerla.

* Hosting gratuito: Podemos alojar nuestro sitio de manera gratuita en
  Github Pages.

### ¿Quiénes están utilizando Jekyll?

* [Jekyll][jekyll]
* [CSS Wizardry][csswizardry]
* [HealthCare.gov][healthcare]
* [Github Pages][github-pages] detrás de escenas utiliza Jekyll para generar los sitios
  estáticos.

## Instalación de Jekyll

Abrimos la terminal y escribimos el siguiente comando para instalar Jekyll (en la Mac). Si tu tienes otro
sistema operativo te recomiendo revisar la [documentación de instalación][jekyll-installation].

{% highlight bash %}
$ sudo gem install jekyll
{% endhighlight %}

## Crear un nuevo proyecto con Jekyll

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
este archivo configuramos a Jekyll para indicarle cómo va a generar nuestro sitio.

El directorio `_includes/` contiene fragmentos de código reutilizables para
incluir en nuestros templates y posts (footer.html, header.html, etc).

El directorio `_layouts/` contiene los templates necesarios para mostrar
nuestras páginas y posts.

En `_posts/` es donde organizaremos nuestras entradas del
blog en archivos de texto con formato markdown.

[Markdown][markdown] es una manera simple de leer/escribir en texto plano documentos web
que después se convertirán en HTML.

Jekyll tiene soporte para Sass y en el directorio `_sass/` se encontrarán todos
nuestros partials. El archivo principal de Sass se encuentra en
`css/main.scss` y es compilado por Jekyll para generar el código css.
Esto es completamente opcional, si alguien quiere trabajar con css puro
puede utilizar solo el directorio `css/` y  escribir css de manera
tradicional.

`index.html` es la portada de nuestro sitio. Pero si tratamos de abrir
ese archivo aun no veremos nada y esto es porque no le hemos dicho a
Jekyll que _genere nuestro sitio estático_.

### Generar el sitio estático con Jekyll

Jekyll tienen un servidor de desarrollo para que podamos mirar nuestro
blog en nuestro equipo local. Para que Jekyll genere y sirva nuestro
sitio ejecutamos el siguiente comando en la terminal. (Debes estar
situado dentro del directorio de tu proyecto Jekyll).

{% highlight bash %}
$ jekyll serve
{% endhighlight %}

Al hacer esto Jekyll generará nuestro sitio estático en el directorio
`_site/` y nos levantará un servidor local en la url `http://127.0.0.1:4000/` observando los cambios que hagamos
en nuestro blog para regenerar nuestro sitio incluso los cambios que
hagamos en nuestros archivos de Sass.

Jekyll tiene un tema por defecto bastante bueno con un post y una página
de ejemplo que podemos utilizar para aprender cómo crear más contenido
en nuestro blog.

![Jekyll Home]({{ site.baseurl }}/assets/jekyll-home.jpg)

Mira un poco la estructura de archivos de tu proyecto Jekyll, notarás
que algunos comienzan con underscore `_`. Estos archivos/directorios son leídos por
Jekyll para generar el sitio estático en el directorio `_site/`, pero no
serán copiados en el sitio final. Todo lo que no comienza con `_` si
es copiado directamente al sitio final como por ejemplo el directorio
`css/`.

Es importante que tengamos esto en cuenta, por ejemplo si queremos incluir un
directorio de assets para guardar imágenes que mas tarde incluiremos en
nuestros posts, creamos un directorio llamado `assets/` en el root del
proyecto:

{% highlight bash %}
▸ _includes/
▸ _layouts/
▸ _posts/
▸ _sass/
▸ _site/
▸ assets/
▸ css/
  _config.yml
  about.md
  feed.xml
  index.html

{% endhighlight %}


## Configuración de Jekyll

Las configuraciones de nuestro proyecto se establecen en el archivo
`_config.yml`. [YAML][yaml-site] es un formato especial para guardar configuraciones
y settings con una sintaxis muy fácil de leer.

{% highlight yaml %}
# Site settings
title: Montalvo Miguelo
email: me@montalvomiguelo.com
description: Web designerd and pixel artist
baseurl: "" # the subpath of your site, e.g. /blog/
url: "http://montalvomiguelo.com" # the base hostname & protocol for your site
twitter_username: montalvomiguelo
github_username:  montalvomiguelo

# Build settings
markdown: kramdown
{% endhighlight %}

Dentro de este archivo se encuentran las variables que ya están
establecidas por Jekyll para configurar nuestro sitio como por ejemplo:

* title
* email
* description
* url

Algunas otras son variables que utiliza el theme que viene por default
en Jekyll:

* twitter_username
* github_username

_Acá más sobre [Jekyll configuration][config-doc]._

Escribe tu propia información. Cada vez que hagamos cambios en `_config.yml` es necesario que detengamos el servidor
`Ctrl+c` y lo levantemos de nuevo con:

{% highlight bash %}
$ jekyll serve
{% endhighlight %}

Haciendo estas modificaciones nuestro sitio es generado con nuestra
propia información:

![My data]({{ site.baseurl }}/assets/my-data.jpg)

### Configuración de permalinks
Por defecto los permalinks de Jekyll son algo como:

```
http://localhost:4000/jekyll/update/2015/09/04/welcome-to-jekyll.html
```

Pero podemos cambiar el estilo de los permalinks con la variable
`permalink`. Jekyll tiene varios estilos para los permalinks, lee la
[documentación sobre este tema][permalinks-doc], a mi me gusta estilo `pretty`
entonces agrego lo siguiente al final de `_config.yml` (Asegúrate de
reiniciar el servidor cada vez que toques este archivo).

{% highlight yaml %}
permalink: pretty
{% endhighlight %}

##YAML Front Matter

Front Matter es una de las características más bonitas que tiene Jekyll.
El YAML Front Matter se usa para guardar información acerca de nuestras páginas por ejemplo:

* `layout` Template que será utilizado para mostrar la página
* `title` El título de la entrada
* `date` Fecha de publicación.
* `categories` Categoría de la entrada
* `author` Autor de la entrada

Incluso puede almacenar valores personalizados como una imagen destacada,
un color de fondo y en general cualquier valor personalizado que necesite tener nuestro
contenido. Te recomiendo dar un vistazo a la [documentación de Front
Matter][frontmatter-doc].

Si abrimos el post que viene de ejemplo con Jekyll, veremos
al inicio del archivo el YAML Front Matter.

{% highlight text %}
{% raw %}
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
{% endraw %}
{% endhighlight %}

El YAML Front Matter siempre debe estar al inicio del archivo, y su
contenido se encuentra dentro del bloque que abre y cierra con 3 guiones
`---`:

{% highlight text %}
---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-09-04 22:04:11
categories: jekyll update
---
{% endhighlight %}

Cada archivo que tenga el YAML Front Matter será rendereado de manera
especial por Jekyll incluso cuando este no tenga ninguna variable como
por ejemplo el archivo `css/main.scss`

{% highlight text %}
---
# Only the main Sass file needs front matter (the dashes are enough)
---
...
{% endhighlight %}

## Crea tu primer post

Para que las entradas del blog sean ordenadas correctamente por
Jekyll, es necesario nombrar los archivos con la siguiente convención:

```
year-month-day-post-name.md
```

Aquí un ejemplo de mi primer post `2015-09-05-jelou.md`. Recuerda debe
estar dentro del directorio `_posts/`:

{% highlight bash %}
▾ _posts/
    2015-09-04-welcome-to-jekyll.markdown
    2015-09-05-jelou.md
{% endhighlight %}

En el contenido de este archivo en primer lugar se encontrará el YAML Front
Matter y después un párrafo de ejemplo.

{% highlight text %}
---
layout: post
title:  "Jelou"
date:   2015-10-05 14:01:58
categories: jekyll update
author: montalvomiguelo
permalink: hello
---
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Phasellus
hendrerit. Pellentesque aliquet nibh nec urna. In nisi neque, aliquet
vel, dapibus id, mattis vel, nisi. Sed pretium, ligula sollicitudin
laoreet viverra, tortor libero sodales leo, eget blandit nunc tortor eu
nibh. Nullam mollis. Ut justo. Suspendisse potenti.
{% endhighlight %}

El post se mostrará de la siguiente manera en el sitio:

![Primer post]({{ site.baseurl }}/assets/primer-post.jpg)

El contenido del post se escribe en Markdown, te dejo los siguientes
enlaces para aprender un poco más sobre el tema:

* [Markdown][markdown]
* [Markdown Cheatsheet][markdown-cheatsheet]

## Páginas
Tenemos una página de ejemplo que viene con Jekyll:

* `about.md`

Las páginas en Jekyll van en el root del proyecto. Podemos agregar las
páginas que necesitemos, por ejemplo `contact.md`

{% highlight text %}
---
layout: page
title: Contact
permalink: /contact/
---

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Asperiores voluptatibus sapiente unde, perferendis similique eum expedita sequi distinctio. Eius eos impedit dolore, fugit ad fugiat totam eveniet quo odit asperiores.
{% endhighlight %}
![Página]({{ site.baseurl }}/assets/pagina.jpg)

Documentación sobre páginas: [Creating pages][creating-pages]

## Liquid

Liquid es un lenguaje de plantillas, su trabajo es procesar los datos que
vienen de nuestro blog y los templates de nuestro tema para generar el
documento HTML final que podemos ver en nuestro navegador.

Liquid es un proyecto open-source creado por Shopify para el desarrollo
de temas. Debido a su eficiencia y facilidad de uso,
este a sido adoptado en otros proyectos como Jekyll, Mixture IO,
Octopress, entre otros.

### Liquid en acción

Con Liquid nosotros vamos a tener acceso a los datos del YAML Front Matter
(title, author, excerpt, etc) e imprimirlos directamente en nuestros
templates.

Ejemplo: (Nuestro primer post)

{% highlight text %}
---
layout: post
title:  "Jelou"
date:   2015-10-05 14:01:58
categories: jekyll update
author: montalvomiguelo
permalink: hello
---
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Phasellus
hendrerit. Pellentesque aliquet nibh nec urna. In nisi neque, aliquet
vel, dapibus id, mattis vel, nisi. Sed pretium, ligula sollicitudin
laoreet viverra, tortor libero sodales leo, eget blandit nunc tortor eu
nibh. Nullam mollis. Ut justo. Suspendisse potenti.
{% endhighlight %}

El template que utilizan los posts es `post.html`:

{% highlight html %}
{% raw %}
---
layout: default
---
<div class="post">

  <header class="post-header">
    <h1 class="post-title">{{ page.title }}</h1>
    <p class="post-meta">{{ page.date | date: "%b %-d, %Y" }}{% if page.author %} • {{ page.author }}{% endif %}{% if page.meta %} • {{ page.meta }}{% endif %}</p>
  </header>

  <article class="post-content">
    {{ content }}
  </article>

</div>
{% endraw %}
{% endhighlight %}

Lo que liquid hace es buscar todos los _tags_ que tiene nuestro
template y reemplazarlos por el valor correspondiente.

Por ejemplo {% raw %}`{{ page.title }}`{% endraw %} será
reemplazado por el título del post que definimos en el YAML Front Matter
`title: "Jelou"`.

En Liquid hay dos tipos de _tags_:

* {% raw %}`{{ }}`{% endraw %} - Para imprimir datos
* {% raw %}`{% %}`{% endraw %} - Para agregar lógica a de programación a nuestros templates
    (condicionales, ciclos, asignar variables, etc). Ejemplo:

    {% highlight text %}
      {% raw %}
        {% if page.author %} • {{ page.author }}{% endif %}
      {% endraw %}
      {% endhighlight %}

Liquid también nos ofrece _filtros_ para manipular la salida de la
información. Se utilizan en combinación con los tags de salida
{% raw %}`{{ }}`{% endraw %} más el carácter `|` seguido del filtro que usaremos. Por
ejemplo el filtro para dar formato a la fecha del post:

{% highlight text %}
{% raw %}
{{ page.date | date: "%b %-d, %Y" }}
{% endraw %}
{% endhighlight %}

```
Sep 12, 2015
```

Te dejo algunos enlaces de interés para aprender un poco más de Liquid:

* [Liquid Documentation][liquid-doc]
* [Getting Started With Liquid][starting-liquid]

* [Variables][vars]

### Templates

Los templates de Jekyll están situados en el directorio `_layouts/`

{% highlight bash %}
▾ _layouts/
    default.html
    page.html
    post.html
{% endhighlight %}

Jekyll permite crear tantos templates como necesites, por defecto
Jekyll te da dos templates:

* `page.html` - Para mostrar las páginas como 'About'
* `post.html` - Para mostrar los posts de nuestro blog

`default.html` Es el layout principal, en él se incluyen
todos los componentes que son constantes en nuestro sitio como la navegación, header, footer,
etc, y dentro de el serán incluidos los demás templates en el _tag_
{% raw %}`{{ content }}`{% endraw %}

{% highlight html %}
{% raw %}
<!DOCTYPE html>
<html>

  {% include head.html %}

  <body>

    {% include header.html %}

    <div class="page-content">
      <div class="wrapper">
        {{ content }}
      </div>
    </div>

    {% include footer.html %}

  </body>

</html>
{% endraw %}
{% endhighlight %}

## Variables

Hay variables que no necesitamos escribir siempre en cada post que
hagamos. El _author_ probablemente solamente será en mi caso
_Montalvo Miguelo_ y el _layout_ que va ser utilizado por mis posts siempre
será _post_ entonces para arreglar esto usaremos _Front Matter defaults_

#Front Matter defaults
Podemos definir valores por defecto para
los bloques _YAML Front Matter_, solamente tenemos que agregar el
siguiente snippet al final del archivo `_config.yml`

{% highlight yaml %}
defaults:
  -
    scope:
      path: "" # an empty string here means all files in the project
      type: "posts" # previously `post` in Jekyll 2.2.
    values:
      layout: "post"
      author: "Montalvo Miguelo"
{% endhighlight %}

En el `scope` le indico que únicamente quiero que se apliquen estos
valores a los `posts`, y en `values` se colocan todos los valores que se
necesiten.

Por cierto este snippet lo tomé de la documentación de Jekyll:

* [Front Matter defaults][front-matter-defaults]


## Borradores (drafts)

Los drafts son útiles cuando estamos escribiendo un post que tal ves no
queremos que sea publicado porque aún está en proceso. Para trabajar
con drafts simplemente creamos el directorio `_drafts/` y ahí movemos
todos los posts que queremos que sean borradores.

{% highlight bash %}
▾ _drafts/
    2015-09-04-welcome-to-jekyll.markdown
{% endhighlight %}

Para poder ver los _drafts_ en nuestro equipo local, debemos levantar el servidor
con el flag `--drafts`

{% highlight bash %}
$ jekyll serve --drafts
{% endhighlight %}

Documentación de drafts: [Working with drafts][drafts]

## Deploy en Githug Pages

Con Github Pages podemos publicar de manera gratuita nuestro Blog
creado con Jekyll. De hecho Github Pages tiene soporte especial para Jekyll,
podemos subir nuestro proyecto sin el directorio `_site/` y este será generado directamente en
Github Pages.

Github Pages trabaja únicamente con archivos estáticos HTML, CSS y
Javascript. No trabaja con ningún lenguaje del lado del servidor.

Puedes publicarlo en Project Pages en el branch `gh-pages` o bien en User Pages. En mi caso lo
hice con User Pages en [montalvomiguelo.github.io][montalvomiguelo] para lo cual  en primer lugar es
necesario crear un repositorio y nombrarlo con la siguiente estructura: `usuario.github.io`

![Repo]({{ site.baseurl }}/assets/repo.jpg)

Ahora solo creamos el repositorio local y lo publicamos en el
repositorio remoto en Github:

{% highlight bash %}
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/montalvomiguelo/montalvomiguelo.github.io.git
git push origin master
{% endhighlight %}

Y en pocos minutos mi blog se ve así:

![Mi blog]({{ site.baseurl }}/assets/mi-blog.jpg)

## Utilizando tu dominio

Puedes configurar tu dominio para que apunte a tu url en Github:

{% highlight text %}
montalvomiguelo.com => montalvomiguelo.github.io
{% endhighlight %}

Pasos:

1.- Crear un archivo llamado `CNAME` en el root del proyecto con el dominio
principal:
{% highlight text %}
montalvomiguelo.com
{% endhighlight %}

El CNAME es utilizado para crear un alias de otro dominio. Por ejemplo
mi url es [montalvomiguelo.github.io][montalvomiguelo] pero quiero un
alias con mi dominio [montalvomiguelo.com][montalvomiguelo].

Recuerda subir estos cambios a tu repositorio remoto:

{% highlight bash %}
git add .
git commit -m "CNAME"
git push origin master
{% endhighlight %}

[What is a CNAME record?][cname]

2.- Apuntar el dominio a los DNS de Github

Para apuntar el dominio a la cuenta de Github tenemos que crear un
`address record` también llamado `A record`.

Un `A record` se utiliza para apuntar un dominio a una o más direcciones
IP.

![A record]({{ site.baseurl }}/assets/a-record.jpg)

Este proceso se describe a detalle en la documentación de
Github Pages:

[Tips for configuring an A record with your DNS provider][dnsgithub]

[markdown]: http://daringfireball.net/projects/markdown/
[jekyll-installation]: http://jekyllrb.com/docs/installation/
[harp-site]: http://harpjs.com/
[middleman-site]: https://middlemanapp.com/
[octopress-site]: http://octopress.org/
[jekyll]: http://jekyllrb.com/
[csswizardry]: http://csswizardry.com/
[healthcare]: https://www.healthcare.gov/
[github-pages]: https://pages.github.com/
[jekyll-configuration]: http://jekyllrb.com/docs/configuration/
[yaml-site]: http://yaml.org/
[permalinks-doc]: http://jekyllrb.com/docs/permalinks/#built-in-permalink-styles
[frontmatter-doc]: http://jekyllrb.com/docs/frontmatter/
[config-doc]: http://jekyllrb.com/docs/configuration/
[markdown-cheatsheet]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[liquid-doc]: https://docs.shopify.com/themes/liquid-documentation/basics
[starting-liquid]: https://webdesign.tutsplus.com/tutorials/getting-started-with-liquid-shopifys-template-language--cms-19747
[vars]: http://jekyllrb.com/docs/variables/
[front-matter-defaults]: http://jekyllrb.com/docs/configuration/#front-matter-defaults
[creating-pages]: http://jekyllrb.com/docs/pages/
[drafts]: http://jekyllrb.com/docs/drafts/
[montalvomiguelo]: http://montalvomiguelo.github.io
[dnsgithub]: https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/
[cname]: https://support.dnsimple.com/articles/cname-record/
[structure]: http://jekyllrb.com/docs/structure/
