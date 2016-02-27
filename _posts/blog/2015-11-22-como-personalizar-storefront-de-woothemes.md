---
title:  "Cómo personalizar Storefront de WooThemes"
date:   2015-11-22 16:30
categories: blog
---
[Storefront][storefront] es un tema de código abierto desarrollado por [WooThemes][woothemes] especialmente
para crear tiendas en línea con WordPress y [WooCommerce][woocommerce] pues ofrece una
integración perfecta con este Plugin.

Storefront es un tema muy ligero y solo nos da lo que necesitamos
__“Una base sólida para crear proyectos con WooCommerce”__; no shortcodes, no sliders
ni funcionalidades raras por defecto que probablemente nunca utilizaríamos
o que ni siquiera sabíamos que estaban ahí.

Para empezar a ver como funciona Storefront, necesitamos:

1. Una instalación nueva de WordPress en tu entorno local de desarrollo
2. Instalar el plugin [WooCommerce][woocommerce]
3. Descargar e importar el [contenido de ejemplo de WooCommerce][dummy-data]
4. Instalar y activar el tema [Storefront][storefront]

Hasta aquí nuestro sitio se mira algo así:

![Storefront Default]({{ site.baseurl }}/assets/storefront-default.jpg)

Storefront viene con dos Page Templates, Full Width y Homepage. Full Width quiere decir
que es una página normal pero no hay sidebar. Homepage es un template ideal para la
página inicial que te muestra los productos, categorías, ofertas, etc.

Entonces crea una página nueva llamada Home (ó el nombre que tu prefieras) y elige que
en ella quieres utilizar el template Homepage. Después de eso ve al menú
_Settings > Reading > Front page displays_, activa la opción _“A static page”_
y elige la página que acabas de crear (Home).

Una vez hecho esto, nuestro sitio ahora se mira así:

![Storefront Homepage]({{ site.baseurl }}/assets/storefront-template-home.jpg)

Observa la estructura de la página inicial y la apariencia por defecto del sitio (colores, tipografía, etc).

La apariencia de Storefront puede cambiarse por medio del Customizer nativo de WordPress:

![Storefront Customizer]({{ site.baseurl }}/assets/storefront-customizer.jpg)

Adicionalmente existen algunas [extensiones de pago para poder personalizar Storefront][storefront-extensions].
Pero realmente podemos hacerlo nosotros mismos utilizando los Hooks que los creadores
de Storefront dejaron a nuestra disposición.

## Storefront Child Theme

Una de las mejores prácticas para personalizar Storefront es creando un Child Theme
donde vamos a guardar todas nuestras modificaciones sin afectar al tema original ni
perder nuestros cambios en futuras actualizaciones de Storefront.

Acá existe un proyecto llamado [Storefront Child Theme][storefront-child] listo para que nosotros empecemos
a hacer nuestras modificaciones. Descárgalo, instálalo y actívalo. Inicialmente lo
vas a ver exactamente igual que el Parent Theme Storefront.

Quiero personalizar el Homepage porque necesito un sidebar, también me gustaría personalizar
la apariencia por defecto, colores y la tipografía. Al final de este Post el resultado será el siguiente:

[![Storefront Customizer]({{ site.baseurl }}/assets/storefront-child.jpg)][demo]

## Estructura de archivos de Storefront Child theme
{% highlight bash %}
▾ assets/
  ▾ sass/
      style.scss*
  functions.php*
  README.md*
  screenshot.png*
  style.css*
{% endhighlight %}

### assets/
Contiene un directorio de sass donde podemos escribir y organizar nuestros estilos personalizados con Sass.

### functions.php
{% highlight php %}
<?php

/**
 * Loads the StoreFront parent theme stylesheet.
 */

function sf_child_theme_enqueue_styles() {

    wp_enqueue_style( 'storefront-child-style', get_stylesheet_directory_uri() . '/style.css', array( 'storefront-style' ) );

}
add_action( 'wp_enqueue_scripts', 'sf_child_theme_enqueue_styles' );

/**
 * Note: DO NOT! alter or remove the code above this text and only add your custom PHP functions below this text.
 */
{% endhighlight %}

Solo tiene una función `sf_child_theme_enqueue_styles()` que se dispara en el
Action Hoook `wp_enqueue_scripts`. Aquí se cargan los estilos del Parent Theme y
después los del Child Theme con la función `wp_enqueue_style()`.

### style.css
{% highlight css %}
/*
Theme Name:     Storefront Child Theme
Theme URI:      https://github.com/stuartduff/storefront-child-theme
Author:         Stuart Duff
Author URI:     http://stuartduff.com
Template:     	storefront
Description:  	This is a blank child theme for WooThemes StoreFront theme
Version:      	1.0.0
License:      	GNU General Public License v2 or later
License URI:  	http://www.gnu.org/licenses/gpl-2.0.html
Text Domain:  	storefront
Tags:         	black, white, light, two-columns, left-sidebar, right-sidebar, responsive-layout, custom-background, custom-colors, custom-header, custom-menu, featured-images, full-width-template, threaded-comments, accessibility-ready
This theme, like WordPress, is licensed under the GPL.
Use it to make something cool, have fun, and share what you've learned with others.
Storefront is based on Underscores http://underscores.me/, (C) 2012-2014 Automattic, Inc.
Resetting and rebuilding styles have been helped along thanks to the fine work of
Eric Meyer http://meyerweb.com/eric/tools/css/reset/index.html
along with Nicolas Gallagher and Jonathan Neal http://necolas.github.com/normalize.css/
FontAwesome License: SIL Open Font License - http://scripts.sil.org/OFL
Images License: GNU General Public License v2 or later
*/
/*
 * Add your own custom css below this text.
 */
{% endhighlight %}
Esta es la hoja de estilos del Child Theme y está vacía, únicamente tiene los metadatos
del tema y referencia al Template Storefront (Parent Theme).

## Estilos personalizados y Google Fonts
En el archivo __functions.php__ agregamos al final las siguientes líneas de código:

{% highlight php %}
<?php
/**
 * Styles / scripts
 */

function sf_child_theme_google_fonts() {
	wp_enqueue_style( 'asap', '//fonts.googleapis.com/css?family=Asap:400,400italic,700,700italic', array( 'storefront-style' ) );
}
add_action( 'wp_enqueue_scripts', 'sf_child_theme_google_fonts' );
{% endhighlight %}

La función `sf_child_theme_google_fonts()` se dispara en el Action Hook
`wp_enqueue_scripts` para cargar la hoja de estilos de Google Fonts.

Después agregamos al final del archivo __assets/sass/style.scss__ los siguientes estilos:
{% highlight scss %}
$base-font: 'Asap', sans-serif;
$header-font: $base-font;

@import 'bourbon/bourbon';
@import "../bower_components/susy/sass/susy";

body,
button,
input,
textarea {
	font-family: $base-font;
}

select {
	font-family: $base-font;
}

h1,
h2,
h3,
h4,
h5,
h6 {
	font-family: $header-font;
}

.page-template-template-homepage-php {
	.content-area {
		@include susy-media(768px) {
			@include span(9 of 12);
		}
	}

	&.left-sidebar {
		.content-area {
			@include susy-media(768px) {
				@include span(last 9 of 12);
			}
		}

		.widget-area {
			@include span(3 of 12);
		}

	}
}

.page-template-template-homepage .site-main {
	padding-top: 0;
}

.sf-child-theme-featured-products {

	.storefront-product-section:last-child {
		border-bottom: 3px solid rgba(0,0,0,.025);
	}

}

.site-content .col-full {
	padding: 4em 0;
}
{% endhighlight %}

Ahora solo falta compilar estos estilos de sass a la hoja de estilos __style.css__ del Child Theme

{% highlight bash %}
$ sass assets/sass/style.scss:style.css
{% endhighlight %}

Nota: Tenemos como dependencias en __style.scss__ a [Bourbon][bourbon] (librería de mixins) y [Susy Grid][susy].
Revisa la documentación de estas pues hay varias maneras de instalarlas. Si no quieres utilizar
Sass, solo copia los [estilos finales del proyecto][style] en tu hoja de estilos __style.css__.

## Filtros para el Customizer
Storefront utiliza el [API de WordPress Customizer][customizer-api] para la personalización del tema.
Empecemos por cambiar el color de fondo para el header que viene por defecto en nuestro Storefront Child Theme.

Agrega lo siguiente al final de tu archivo __functions.php__:

{% highlight php %}
<?php

/**
 * Customizer default color tweaks
 */

function sf_child_theme_color_cerise_red( $color ) {
	$color = '#E23D54';
	return $color;
}
add_filter( 'storefront_default_header_background_color', 'sf_child_theme_color_cerise_red' );
{% endhighlight %}

Básicamente estamos agregando un filtro con el Hook
`storefront_default_header_background_color`. En el atamos la función
`sf_child_theme_color_cerise_red()` que únicamente se encarga de retornar el
color rojito bonito que quiero.

Este filtro está siendo aplicado en el Parent Theme de la siguiente manera:

{% highlight text %}
Searching 114 files for "apply_filters( 'storefront_default_header_background_color'"

/wp-content/themes/storefront/inc/customizer/controls.php:
  130  		 */
  131  		$wp_customize->add_setting( 'storefront_header_background_color', array(
  132: 			'default'           => apply_filters( 'storefront_default_header_background_color', '#2c2d33' ),
  133  			'sanitize_callback' => 'storefront_sanitize_hex_color',
  134  			'transport'			=> 'postMessage',
{% endhighlight %}

En la línea __132__ del archivo __/wp-content/themes/storefront/inc/customizer/controls.php__
verás que nos dejaron un: `apply_filters( 'storefront_default_header_background_color', '#2c2d33' ),`

Esto quiere decir que el color de fondo en el header va ser `#2c2d33` por defecto, al
menos de que nosotros lo modifiquemos agregando un filtro llamado
`storefront_default_header_background_color`.

En el Child Theme es donde nosotros creamos ese filtro y retornamos el color que
nosotros queremos. Recuerda que los filtros se encargan de recibir un valor y retornar
el valor modificado.

En el archivo __/wp-content/themes/storefront/inc/customizer/controls.php__ verás que
los settings vienen con algún color por defecto. Todos estos settings tienen la
opción de ser modificados a través de un filtro con `apply_filters()`.

Nosotros tenemos que implementar los filtros que necesitemos con
`add_filter()` en nuestro Child Theme para modificar estos valores.

Entonces agregamos al final del archivo __functions.php__ los demás filtros para
nuestro Storefront Child Theme.

{% highlight php %}
<?php

function sf_child_theme_color_white( $color ) {
	$color = '#ffffff';
	return $color;
}
add_filter( 'storefront_default_header_text_color', 'sf_child_theme_color_white' );

function sf_child_theme_color_mine_shaft( $color ) {
	$color = '#3F3F3F';
	return $color;
}
add_filter( 'storefront_default_button_background_color', 'sf_child_theme_color_mine_shaft' );

function sf_child_theme_color_peter_river( $color ) {
	$color = '#3498db';
	return $color;
}
add_filter( 'storefront_default_button_alt_background_color', 'sf_child_theme_color_peter_river' );
add_filter( 'storefront_default_accent_color', 'sf_child_theme_color_peter_river' );
add_filter( 'storefront_default_footer_link_color', 'sf_child_theme_color_peter_river' );
{% endhighlight %}

## Personalizando el Layout de Homepage
Creamos un archivo llamado __template-homepage.php__ en nuestro
Child Theme con el siguiente contenido:

{% highlight php %}
<?php
/**
 * Customize the stock Storefront homepage template to include the sidebar and the sf_child_theme_before_homepage_content hook.
 *
 * Template name: Homepage
 *
 * @package storefront
 */

get_header(); ?>

	<div class="sf-child-theme-featured-products site-main">
		<?php
		/**
		 * @hooked storefront_homepage_content - 10
		 * @hooked storefront_featured_products - 20
		 */
		do_action( 'sf_child_theme_before_homepage_content' ); ?>
	</div>

	<div id="primary" class="content-area">
		<main id="main" class="site-main" role="main">

			<?php
			/**
			 * @hooked storefront_recent_products - 30
			 * @hooked storefront_popular_products - 50
			 * @hooked storefront_on_sale_products - 60
			 */
			do_action( 'homepage' ); ?>

		</main><!-- #main -->
	</div><!-- #primary -->

	<?php do_action( 'storefront_sidebar' ); ?>

<?php get_footer(); ?>
{% endhighlight %}

En este archivo estamos sobreescribiendo lo que hacia el __template-homepage.php__ del Parent Theme.

Le agregamos un `div` con la clase `.sf-child-theme-featured-products`. También agregamos el
sidebar con la función `<?php do_action( 'storefront_sidebar' );`.

En la sección `.sf-child-theme-featured-products` se encuentra la función
`do_action( 'sf_child_theme_before_homepage_content' );` y quiere decir que aquí me va a imprimir
todo lo que esté atado en el Hook `sf_child_theme_before_homepage_content`.
En este caso le ataremos dos Action Hooks: `storefront_homepage_content` y `storefront_featured_products`.

Los Hooks `homepage` y `storefront_sidebar` ya vienen por defecto en el Parent Theme.

En el Hook `homepage` están atadas todas las funciones que nos imprimen las diferentes
secciones en el Homepage (Líneas __52-57__):

{% highlight text %}
Searching 115 files for "add_action( 'homepage'"

/wp-content/themes/storefront/inc/structure/hooks.php:
   50   * @see  storefront_on_sale_products()
   51   */
   52: add_action( 'homepage', 'storefront_homepage_content',		10 );
   53: add_action( 'homepage', 'storefront_product_categories',	20 );
   54: add_action( 'homepage', 'storefront_recent_products',		30 );
   55: add_action( 'homepage', 'storefront_featured_products',		40 );
   56: add_action( 'homepage', 'storefront_popular_products',		50 );
   57: add_action( 'homepage', 'storefront_on_sale_products',		60 );
   58
   59  /**
{% endhighlight %}

En el Hook `storefront_sidebar` está atada una función solo para incluir el sidebar (Línea __240__).
{% highlight text %}
Searching 115 files for "storefront_get_sidebar()"

/wp-content/themes/storefront/inc/structure/template-tags.php:
  238  	 * @since 1.0.0
  239  	 */
  240: 	function storefront_get_sidebar() {
  241  		get_sidebar();
  242  	}
{% endhighlight %}

El Hook `sf_child_theme_before_homepage_content` es el único nuevo que agregamos nosotros
para el Child Theme, ahí nosotros debemos indicarle que es lo que va pasar.

## Acciones y Filtros para personalizar Homepage
Agregamos las siguientes líneas al final de __functions.php__:

{% highlight php %}
<?php

/**
 * Adjust the storefront homepage template layout
 */

function sf_child_theme_homepage_layout() {
	remove_action( 'homepage', 'storefront_homepage_content', 10 );
	remove_action( 'homepage', 'storefront_featured_products', 40 );
	remove_action( 'homepage', 'storefront_product_categories', 20 );

	add_action( 'sf_child_theme_before_homepage_content', 'storefront_homepage_content', 10 );
	add_action( 'sf_child_theme_before_homepage_content', 'storefront_featured_products', 20 );
}
add_action( 'init', 'sf_child_theme_homepage_layout' );
{% endhighlight %}

En primer lugar atamos la función `sf_child_theme_homepage_layout` en el Hook `init`.
En esa función es donde empezamos a personalizar lo que queremos mostrar en el Homepage.

Le hemos indicado que no queremos que imprima las siguientes secciones en el Hook
`homepage` con la funcion `remove_action()`

* storefront_homepage_content
* storefront_featured_products
* storefront_product_categories

Después le indicamos que es lo que queremos imprimir en el Hook
`sf_child_theme_before_homepage_content` con la `función add_action()`

* storefront_homepage_content
* storefront_featured_products

Por último quiero que en mi Homepage se muestran filas con solamente 3 productos.
Entonces agregamos los siguientes Filtros al final de __functions.php__:

{% highlight php %}
<?php

/**
 * Homepage
 */

function sf_chilld_theme_product_columns( $args ) {
	$args['limit'] = 3;
	$args['columns'] = 3;
	return $args;
}
add_filter( 'storefront_featured_products_args', 'sf_chilld_theme_product_columns' );
add_filter( 'storefront_recent_products_args', 'sf_chilld_theme_product_columns' );
add_filter( 'storefront_popular_products_args', 'sf_chilld_theme_product_columns' );
add_filter( 'storefront_on_sale_products_args', 'sf_chilld_theme_product_columns' );
{% endhighlight %}

Estos filtros sirven para modificar los argumentos (arreglo) que están siendo pasados a
las funciones que se encargan de imprimir los productos. (Estas funciones ya están
escritas en el Parent Theme)

Por ejemplo el `filtro storefront_featured_products_args` en la
línea __98__ del archivo __/wp-content/themes/storefront/inc/structure/template-tags.php__

{% highlight text %}
Searching 115 files for "apply_filters( 'storefront_featured_products_args"

/wp-content/themes/storefront/inc/structure/template-tags.php:
   96  		if ( is_woocommerce_activated() ) {
   97
   98: 			$args = apply_filters( 'storefront_featured_products_args', array(
   99  				'limit' 			=> 4,
  100  				'columns' 			=> 4,

{% endhighlight %}

Y así terminamos de personalizar un poco nuestro Storefront Child Theme.
Como puedes ver es muy sencillo hacerlo a través de los Hooks que nos deja disponibles Storefront.

Nuestra imaginación es el único límite para personalizar nuestro tema
como nosotros queramos. Si te gustó este Post compártelo y si tienes
alguna duda, déjame tus comentarios será un gusto poder ayudarte.

[woothemes]: http://www.woothemes.com/
[woocommerce]: http://www.woothemes.com/woocommerce/
[dummy-data]: https://docs.woothemes.com/document/importing-woocommerce-dummy-data/
[storefront]: http://www.woothemes.com/storefront/
[storefront-extensions]: http://www.woothemes.com/product-category/storefront-extensions/
[storefront-child]: https://github.com/stuartduff/storefront-child-theme
[bourbon]: http://bourbon.io/
[susy]: http://susydocs.oddbird.net/en/latest/install/
[style]: https://github.com/montalvomiguelo/Storefront-child/blob/master/style.css
[demo]: http://wp.montalvomiguelo.com/storefront-child/
[customizer-api]: https://codex.wordpress.org/Theme_Customization_API
