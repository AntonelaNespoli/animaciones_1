# Ejercicio Práctico con JavaScript

## Estableciendo multiples valores para la propiedades animation

Las propiedades de la versión larga de `animation` pueden aceptar múltiples valores, separados por comas. *Esta característica podemos usarla cuando queremos aplicar múltiples animaciones en una sola regla*, y establecer por separado duration, iteration-count, etc. para diferentes animaciones. Vamos a ver algunos ejemplos para explicar las diferentes combinaciones:

En el primero, tenemos tres nombres de animación establecidos, pero solo una duración (duration) y número de iteraciones (iteration-count). En este caso, a las tres animaciones se les da la misma duración y número de iteraciones:

    animation-name: fadeInOut, moveLeft300px, bounce;
    animation-duration: 3s;
    animation-iteration-count: 1;

En el segundo, tenemos tres valores establecidos en las tres propiedades. En este caso, cada animación se ejecuta con los valores correspondientes en la misma posición en cada propiedad, *así por ejemplo fadeInOut tiene una duración de 2.5s y 2 iteraciones, etc.*

    animation-name: fadeInOut, moveLeft300px, bounce;
    animation-duration: 2.5s, 5s, 1s;
    animation-iteration-count: 2, 1, 5;

En el tercer caso, hay tres animaciones especificadas, pero solo dos duraciones y número de iteraciones. En los casos en donde no hay valores suficientes para dar un valor separado a cada animación, los valores se repiten de inicio a fin. *Así por ejemplo, fadeInOut obtiene una duración de 2.5s y moveLeft300px obtiene una duración de 5s. Ahora tenemos asignados todos los valores de duración disponibles, así que empezamos desde el inicio de nuevo - por lo tanto bounce  tiene una duración de 2.5s. El número de iteraciones (y cualquier otra propiedad que especifiques) será asignados de la misma forma.*

    animation-name: fadeInOut, moveLeft300px, bounce;
    animation-duration: 2.5s, 5s;
    animation-iteration-count: 2, 1;

## Usando eventos de animación

Podemos tener un control mayor sobre las animaciones (así como información útil sobre ellas) haciendo uso de eventos de animación. Dichos eventos, representados por el objeto `AnimationEvent` , se pueden usar para detectar cuándo comienza la animación, cuándo termina y cuándo comienza una iteración. Cada evento incluye el momento en el que ocurrió, así como el nombre de la animación que lo desencadenó.

Tomando como base el ejercicio anterior, vamos a modificarlo para recoger información sobre cada evento cuando suceda, y asi podremos echar un vistazo a cómo funcionan.

### ***Agregando CSS***

Empezamos creando el CSS para la animación.

    .slidein {
        animation-duration: 3s;
        animation-name: slidein;
        animation-iteration-count: 3;
        animation-direction: alternate;
    }

    @keyframes slidein {
        from {
            margin-left:100%;
            width:300%
        }

        to {
            margin-left:0%;
            width:100%;
        }
    }

### ***Añadiendo detectores de eventos a la animación***

Usaremos un poco de Javascript para escuchar los tres posibles eventos de animación. Este código configura nuestros detectores de eventos (event listeners); los llamamos cuando el documento carga por primera vez para configurar todo.

    var e = document.getElementById("watchme");
    e.addEventListener("animationstart", listener, false);
    e.addEventListener("animationend", listener, false);
    e.addEventListener("animationiteration", listener, false);
    e.className = "slidein";

Esta es la forma estándar de detectar eventos en Javascript, si quieren conocer más detalles sobre cómo funciona la detección de eventos, consulta la documentación de [element.addEventListener()](https://developer.mozilla.org/es/docs/Web/API/EventTarget/addEventListener).

La última línea pone la clase `slidein` al elemento para comenzar la animación. ¿Por qué?. Porque que el evento *animationstart* se dispara cuando comienza la animación y, en nuestro caso, esto sucedería antes de que nuestro código se hubiera ejecutado y no podríamos crear los detectores de eventos. Para evitarlo, creamos los detectores de eventos antes y añadimos la clase al elemento para iniciar la animación.

### ***Recibiendo los eventos***

Los eventos, al irse disparando, llamarán a la función `listener()`.

    function listener(e) {
        var l = document.createElement("li");
        switch(e.type) {
            case "animationstart":
            l.innerHTML = "Iniciado: tiempo transcurrido " + e.elapsedTime;
            break;
            case "animationend":
            l.innerHTML = "Finalizado: tiempo transcurrido " + e.elapsedTime;
            break;
            case "animationiteration":
            l.innerHTML = "Nueva iteración comenzó a los " + e.elapsedTime;
            break;
        }
        document.getElementById("output").appendChild(l);
    }

Este código también es muy sencillo. Miramos en event.type para saber qué tipo de evento se ha disparado y, en función del tipo de evento, añadimos su correspondiente texto al elemento `<ul>` que usaremos para registrar la actividad de nuestros eventos.

El resultado, si todo ha ido bien, será algo parecido a esto:

    Iniciado: tiempo transcurrido 0
    Nueva iteración comenzó a los 3.01200008392334
    Nueva iteración comenzó a los 6.00600004196167
    Finalizado: tiempo transcurrido 9.234000205993652

Fijémonos en que después de la iteración final de la animación, el evento `animationiteration` no se envía, en su lugar se lanza `animationend`.

### ***El HTML***

Solo nos falta mostrar el código HTML necesario para mostrar el ejemplo en la página, incluyendo la lista en la que el script irá insertando la información de los eventos que se vayan disparando.

    <h1 id="watchme">Watch me move</h1>
    <p>This example shows how to use CSS animations to make <code>H1</code>
    elements move across the page.</p>
    <p>In addition, we output some text each time an animation event fires,
    so you can see them in action.
    </p>
    <ul id="output">
    </ul>

**Fuente: [Usando animaciones en CSS](https://developer.mozilla.org/es/docs/Web/CSS/CSS_Animations/Usando_animaciones_CSS)**
