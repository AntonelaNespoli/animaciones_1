# Ejercicio Práctico

## Haciendo que un texto se deslice por la ventana del navegador

Este sencillo ejemplo da estilos al elemento `<p>` para que el texto se deslice por la pantalla entrando desde el borde derecho de la ventana del navegador.

Animaciones como esta pueden hacer que la página se vuelva más ancha que la ventana del navegador. Para evitar este problema, pon el elemento que será animado en un contenedor y agrega `overflow:hidden` en el contenedor.

    p {
    animation-duration: 3s;
    animation-name: slidein;
    }
    @keyframes slidein {
        from {
            margin-left: 100%;
            width: 300%
        }

        to {
            margin-left: 0%;
            width: 100%;
        }
    }

El estilo del elemento `<p>` especifica, a través de la propiedad `animation-duration`, que la animación debe durar 3 segundos desde el inicio al fin y que el nombre de los `@keyframes` que definen los fotogramas de la secuencia de la animación es *"slidein"*.

Si queremos añadir algún estilo personalizado sobre el elemento `<p>` para usarlo en navegadores que no soporten animaciones CSS, también podemos incluirlos. En nuestro ejemplo, no queremos ningún otro estilo personalizado diferente al efecto de la animación.

Los fotogramas se definen usando la regla `@keyframes`. En nuestro ejemplo, tenemos solo dos fotogramas. El primero de ellos sucede en el 0% (hemos usado su alias **from**). Aquí, configuramos el margen izquierdo del elemento, poniendolo al 100%  (es decir, en el borde derecho del elemento contenedor), y su ancho al 300% (o tres veces el ancho del elemento contenedor). Esto hace que en el primer fotograma de la animación tengamos el encabezado fuera del borde derecho de la ventana del navegador.

El segundo (y último) fotograma sucede en el 100% (hemos usado su alias **to**). Hemos puesto el margen derecho al 0% y el ancho del elemento al 100%. Esto produce que el encabezado, al finalizar la animación, esté en el borde derecho del área de contenido.

    <p>The Caterpillar and Alice looked at each other for some time in silence:
    at last the Caterpillar took the hookah out of its mouth, and addressed
    her in a languid, sleepy voice.</p>

## Añadiendo otro fotograma

Vamos a añadir otro fotograma a la animación de nuestro ejemplo anterior. Pongamos que queremos que el tamaño de fuente del encabezado aumente a medida que se mueve durante un tiempo y que después disminuye hasta su tamaño original. Esto es tan sencillo como añadir este fotograma:

    75% {
    font-size: 300%;
    margin-left: 25%;
    width: 150%;
    }

Esto le dice al navegador que en el 75% de la secuencia de la animación, el encabezado tiene un margen izquierdo del 25%, un tamaño de letra del 200% y un ancho del 150%.

## Haciendo que se repita

Para hacer que la animación se repita, solo hay que usar la propiedad `animation-iteration-count` e indicarle cuántas veces debe repetirse. En nuestro caso, usamos `infinite` para que la animación se repita indefinidamente:

    p {
        animation-duration: 3s;
        animation-name: slidein;
        animation-iteration-count: infinite;
    }

## Haciendo que se mueva hacia adelante y hacia atras

Hemos hecho que se repita, pero queda un poco raro que salte al inicio de la animación cada vez que ésta comienza. Queremos que se mueva hacia adelante y hacia atrás en la pantalla. Esto lo conseguimos fácilmente indicando que animation-direction es alternate:

    p {
        animation-duration: 3s;
        animation-name: slidein;
        animation-iteration-count: infinite;
        animation-direction: alternate;
    }

## Usando la versión abreviada de animation

La versión abreviada animation es usado para ahorrar espacio. Por ejemplo, la regla que hemos usado en este ejercicio para el elemento `<p>`:

    p {
        animation-duration: 3s;
        animation-name: slidein;
        animation-iteration-count: infinite;
        animation-direction: alternate;
    }

Esto podriamos remplazarlo por: 

    p {
        animation: 3s infinite alternate slidein;
    }