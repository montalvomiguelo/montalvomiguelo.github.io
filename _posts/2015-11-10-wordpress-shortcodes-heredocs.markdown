---
title:  "WordPress y Shortcodes bonitos con Heredocs"
date:   2015-11-10 13:30
categories: php wordpress
---
Los Heredocs son de gran utilidad cuando necesitamos incluir variables
en Snippets de Html. Por ejemplo en la creación de
_shortcodes_.

Para esta tarea es común en el desarrollo WordPress encontrarnos con
cadenas concatenadas de la siguiente manera:

{% highlight php %}
<?php

// [caption]My Caption[/caption]
function caption_shortcode( $atts, $content = null ) {
    return '<span class="caption">' . $content . '</span>';
}
add_shortcode( 'caption', 'caption_shortcode' );
{% endhighlight %}

Donde al utilizar el shortcode obtendrías algo como:

{% highlight html %}
<span class="caption">My Caption</span>
{% endhighlight %}

Pero ... ¿Qué pasaría cuando nuestro Snippet es más grande?. Seguramente
concatenando cadenas sería muy complicado construir un Snippet más
grande.

## Heredocs al rescate
Los Heredocs se comportan como las cadenas de texto en comillas dobles,
las variables que estén escritas dentro de la cadena van a ser
interpretadas.

Los Heredocs comienzan con tres picos `<<<EOT` donde `EOT` es un
identificador y se cierra con el nombre del identificador más punto y
coma `EOT;`.

{% highlight php %}
<?php

$nombre = Carlos;
echo "Hola me llamo $nombre.";

echo <<<EOT
Hola me llamo $nombre.
EOT;

{% endhighlight %}

Entonces si tienes un Shortcode con un Snippet de Html mucho más grande
puedes utilizar los Heredocs a tu favor para resolverlo de una manera
más elegante:

{% highlight php %}
<?php

// [widget link="http://st.com" image="image.jpg" title="Cool" body="Any text"]
function my_widget($atts, $content = null) {
    extract( shortcode_atts( array(
       'link' => '',
       'image' => '',
       'title' => '',
       'body' => '',
    ), $atts ) );
    return <<<EOT
<div class="home_widget widget_text">
	<div class="textwidget">
		<a href="$link">
			<img src="$image" alt="alt">
		</a>
		<h2>$title</h2>
		<p>$body</p>
		<a href="$link" class="more-link">Read More</a>
	</div>
</div>
EOT;
}
add_shortcode("widget", "my_widget");
{% endhighlight %}
