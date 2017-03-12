---
title:  "¿Qué son los Hooks de WordPress?"
date:   2015-11-15 22:40
categories: blog
tags: WordPress
color: "#c2a2f5"
---
Son un término genérico para referirnos a un evento que sucede en WordPress
donde nosotros podemos agregar nuestro propio código o modificar lo que WordPress
hace por defecto. Existen dos tipos de Hooks en WordPress, _Actions_ y _Filters_.

## Actions

Es un Hook que se dispara en un punto determinado durante la ejecución
de WordPress y nos permite hacer algo en ese momento.

Por ejemplo en el Hook `wp_enqueue_scripts` es donde nosotros aprovechamos
para cargar los estilos y scripts de nuestro sitio.

[Listado de los Action Hooks nativos de WordPress.][action-hooks]

## Filters

Es un Hook que nos permite manipular y __siempre retornar un valor__.

Por ejemplo cuando necesitamos establecer la longitud para el excerpt
de un post lo hacemos con el filtro `excerpt_length`.

[Listado de los Filter Hooks nativos de WordPress.][filter-hooks]

## Funciones para Filter Hooks

### [add_filter()][add-filter]
{% highlight php %}
<?php add_filter( $tag, $function_to_add, $priority, $accepted_args ); ?>
{% endhighlight %}

Esta función nos permite agregar nuestra propia función para un Filter
Hook.

{% highlight php %}
<?php

function custom_excerpt_length( $length ) {
	return 20;
}
add_filter( 'excerpt_length', 'custom_excerpt_length', 999 );
{% endhighlight %}

En este ejemplo atamos la función `custom_excerpt_length` en el Filter Hook
`excerpt_length` para retornar el valor __20__.
Este valor está siendo utilizado por WordPress de la siguiente manera
para establecer la longitud del excerpt de un post.

{% highlight bash %}
/wphooks.dev/wp-includes/formatting.php:
 2813  		 * @param int $number The number of words. Default 55.
 2814  		 */
 2815: 		$excerpt_length = apply_filters( 'excerpt_length', 55 );
 2816  		/**
 2817  		 * Filter the string in the "more" link displayed after a trimmed excerpt.

1 match in 1 file
{% endhighlight %}

WordPress por default le está pasando un valor de __55__ al filtro `excerpt_length`.
Si nosotros no hacemos nada en ese filtro por default la longitud será __55__.

### [apply_filters()][apply-filters]

{% highlight php %}
<?php apply_filters( $tag, $value, $var ... ); ?>
{% endhighlight %}


Esta función permite modificar un valor que pasa a través de un filtro. Ejemplo:

__Pepito__ pasa a través del filtro `rocks_filter`

{% highlight php %}
<?php

echo apply_filters( 'rocks_filter', 'Pepito' );
{% endhighlight %}

Si no existe el filtro aún, el valor que se imprime es simplemente __Pepito__.
Si lo dejamos así quiere decir que le estamos dando la oportunidad a otros
desarrolladores de implementar su propio filtro con `add_filter()` para modificar
este valor.

{% highlight php %}
<?php

function rocks_callback( $name ) {
	return "$name Rocks!!!";
}
add_filter( 'rocks_filter', 'rocks_callback' );

echo apply_filters( 'rocks_filter', 'Pepito' );
{% endhighlight %}

Cuando implementamos el filtro `rocks_filter` el valor que se imprime es __Pepito Rocks!!!__.

Es importante que cuando tu veas `apply_filters( ‘filter_name’, array(‘color’ => ‘#333333’)  );`
te fijes en los argumentos que están pasando por ese filtro y los
recibas de esa manera cuando estás creando tu filtro con `add_filter()`.

### [remove_filter()][remove-filter]

{% highlight php %}
<?php remove_filter( $tag, $function_to_remove, $priority ); ?>
{% endhighlight %}

Quita una función que está atada a un Filter Hook:

{% highlight php %}
<?php

remove_filter( 'the_content', 'do_shortcode', 11);
{% endhighlight %}

## Funciones para Action Hooks

### [add_action()][add-action]

{% highlight php %}
<?php add_action( $hook, $function_to_add, $priority, $accepted_args ); ?>
{% endhighlight %}

Permite ejecutar una función que atamos a un Action Hook.
Por ejemplo cuando cargamos los estilos que utiliza un theme:

{% highlight php %}
<?php

function theme_styles() {

	wp_enqueue_style( 'google_fonts', 'https://fonts.googleapis.com/css?family=Open+Sans' );

}
add_action( 'wp_enqueue_scripts', 'theme_styles' );
{% endhighlight %}

### [remove_action()][remove-action]

{% highlight php %}
<?php remove_action( $tag, $function_to_remove, $priority ); ?>
{% endhighlight %}

Quita una función que está atada a un Action Hook.

{% highlight php %}
<?php

remove_action( 'wp_enqueue_scripts', 'theme_styles' );
{% endhighlight %}

### [do_action()][add-action]

{% highlight php %}
<?php do_action( $tag, $arg ); ?>
{% endhighlight %}

Ejecuta una función que está atada a un Action Hook:

{% highlight php %}
<?php

function cool_dump_array_callback( $arr ) {
    echo '<pre>';
    print_r( $arr );
    echo '</pre>';
}
add_action( 'cool_dump_array', 'cool_dump_array_callback' );

do_action( 'cool_dump_array', array( 'name' => 'montalvo', 'lastname' => 'miguelo' ) );
{% endhighlight %}

Con `add_action()` podemos dar la opción a otros desarrolladores de
implementar su propia acción de manera similar como hacemos con `apply_filters()`.

[filter-hooks]: http://codex.wordpress.org/Plugin_API/Filter_Reference
[action-hooks]: http://codex.wordpress.org/Plugin_API/Action_Reference
[add-filter]: https://wordpress.org/search/add_filter
[apply-filters]: https://codex.wordpress.org/Function_Reference/apply_filters
[remove-filter]: https://codex.wordpress.org/Function_Reference/remove_filter
[add-action]: https://wordpress.org/search/add_action
[remove-action]: https://codex.wordpress.org/Function_Reference/remove_action
[do-action]: https://codex.wordpress.org/Function_Reference/do_action
