Viernes 15/julio/2022
    En esta estapa de utilizar grid estamos implementando reemplazar las imágenes desplegables de cada contenedor temático por un contenedor de cards con efecto flip en 3D.

    La incidencias que tuvimos fueron las mismas de la etapa de Flexbox con tener la barra de navegación superior sin ocupar todo el width al aplicar position:fixed.

    Nos falta resolver en darle estilo a los botones comando del audio, eliminar el espacio en blanco que ocupa el header cuando se scrollea, eliminar el espacio vacío entre el bottom del header y el top del main, y resolver la distribución del footer, sin contar el trabajo casi total del resto de las páginas.


Sábado 16/julio/2022
    Con ayuda de Timoteo logramos completar la barra superior fija en todo su ancho al poner a la propiedad width:100%, estas propiedades se heredan, de hecho todas las de box modeling nada se heredan (referidas a fuentes, alineaciones, backgrounds).

    Luego el background video logramos sacarlo del bottom cambiando la propiedad position: de fixed a absolute, consecuentemente el height del scroll del contenedor de los subtitulos necesitabamos cambiarlo por lo siguiente ya que al tener un determinado height se genera una segunda barra de scroll del contenedor en cuestión entonces necesitamos configurarla a la medida de la imagen video de fondo con lo siguiente aprox:
.main__scroll{
    min-height: 800px;
    max-height: 800px;
    overflow-y: scroll;   
}
Y aún sigue sin estar perfectamente coincidente con el borde inferior del vidego background.
    Y para ocultar la barra scroll lo siguiente:

.main__scroll::-webkit-scrollbar{
    width:0px;
}

    Una observación no menor es que las visualizaciones por medio de un hosting como Vercel desde repositorio hay detalles de visualización que nos son iguales a cuando pruebo el mismo proyecto con el Go Live de VSC, desconozco cual es la razón.

    Por lo que me comentaron en Discord sobre el smooth es que sólo funciona para navegaciones entre secciones de una sola página, la misma donde se encuetra la navegación, o sea con hipervínculos linkeados con id en el href.

    Finalizando en madrugada de Domingo al borrar las medidas del height en las grids no se modifican en nada las dimensiones de la página, esto lo descubrimos al querer reducir de tamaño el footer. Queda pendiente corregir el footer a mas chico.

18/julio/2022
    Utilizamos transformaciones anteriormente y ahora agregamos el uso de animaciones desde repositorio https://animate.style/ , podemos pasar la hoja de estilo en linea dentro del head por link y luego colocar a cada elemento el nombre de la animación buscada y en la hoja de estilo configurada como por ejemplo :hover hacer el llamado del mismo nombre de la animación y subsiguientes configuraciones de la misma.
    
21/julio/2022
    Comenzando con implemetar media query nos encontramos con que toma una orden solo a los 768px cambiar el tamaño pero otra media con 48px no la toma.
    
25/julio/2022
    Arreglando distintos puntos:
>>>    Aparecía visible la barra de dos scroll, uno de ellos fué porque implementamos la propiedad overflow-y:scroll para lograr ocultar el contenedor desplegado en un determinado tamaño, así que al mismo contenedor le aplicamos el siguiente estilo 
        body::-webkit-scrollbar{
    width:0px;
    }
        con ello dejamos de ver el scroll que sigue estando pero sin espacio físico visible

>>>      Como el contenedor de todas las cards con imágenes y los subtitulos desplazaban el contenedor de about lo pusimos en 
    .main__scroll{
    min-height: 90vh;
    max-height: 90vh;
    overflow-y: scroll;
    }

        con ello quedó más ordenado el lugar del about pero al achicar el viewport sigue escalandose en forma no deseada el video aún usando
    .video{
    min-height: 100vh;
    max-height: 100vh;
    min-width: 100vw;
    max-width: 100vw;
    position: absolute;
    z-index: -1;
    opacity: 0.25;
    object-fit: cover;
    }
>>>    En intentos sin resultado por lograr modificar automáticamente el height de los contenedores desplegables con propiedades de ttransition: all   descubrimos de la mano de un colega experimentado de tantos a consultar, el asunto de que por sentido común queremos aplicar unidades relativas a los valores que manejará transition, y no gente, transition sólo admite valores absolutos únicamente, intentando usar equivocadamente los valores auto ó vh ó vw ó % y en absoluto funcionaban, era por esta razón no de la porque estaba mal estructurado mi hoja de estilo y ó html, aunque puede estarlo en otras partes.

>>>     Arrancamos de lleno con las media querys y nos enfocamos primero en el footer con mayor cantidad de contenedores y elementos a organizar en responsive mobile 320x480px orientación portait.
    La principal dificultad que la entendimos al final fué de que el height del contenedor footer estaba en base a una grid en 3 rows por lo que estas estaban en medidas relativas en fraccion y logramos reducir el aparente margin-bottom con modificar la fraccion de los siguiente:
    .footer {
        grid-template-rows: 1fr .75fr 0.18fr;;
    }
    Nos queda del footer resolver el tamaño de los iconos sociales que en principio mal lo realizamos desde el HTML.
    
    Siguiendo por eliminar la existencia de grid innecesario en el body ajustamos el ancho final con medidas vw y quedó bien, todavía sobresalen ciertos contenedores con aparentes medidas relativas en %.

    Acabamos de solucionar el escalado del video sin considerar el height del contenedor con el siguiente código:

    .video{
    display: flex;    
    position: absolute;
    z-index: -1;
    opacity: 0.25;
    min-width: 100vw;  (estos width al sacarlos no toma el centrado)
    max-width: 100vw;
    justify-content: center;
    object-fit: cover;
    }

    Terminamos el día con que el haber usado medidas con vw fue un error total, en absoluto cuando se escala a otros viewport conserva proporciones.
    
28/julio/2022
    Finalmente arreglamos el responsive y varias partes más de las que rompen el mismo.
    En principio con la ayuda del tuto Timoteo vimos que el uso de grid rompía el responsive en parte agregando un espacio en blanco al lateral derecho del header y main, anulando las GRID se arreglaba totalmente.

    Entonces busqué eliminar las grid de el footer en donde sólo se utilizaba como requisito de entrega en usar grids, y puse grid al contenedor general que es main, aún así persistía la misma incidencia.

    Con la ayuda de un colega en discord, en hacer pruebas al señalarme el uso del tamaño de los íconos que tal vez era el problema, probamos con eliminar el margin de los mismos y reeplazarlo por padding del mismo tamaño y se solucionó parcialmente, persiste un ancho la mitad.

    Aparentemente el reset del html no actúa sobre el margin el cual suma al desborde del contenedor padre cuando se implementa grids.

    La observación al momento es que si desactivamos o no usamos grid, se soluciona el espacio vacío de la derecha vertical, y persiste una zona vacia sin conectar el video con el footer.

4/agosto/2022
    Encontramos el error tan simple sobre la falla con grid, y se debia a que en la etiqueta <main> habiamos agregado una s de más, y por ello no la reconocía como tal, aún cuando el editor vsc no reportaba ningún problema.

    Entramos en la fase de implementar Bootstrap, hemos elegido hacer de la barra de navegación superior un elemento con bootstrap para reemplazarla totalmente agregando la funcionalidad de una burguer con las rutas de hipervínculos a las distintas secciones de la página.

    Estamos por decidir el reemplazo y adaptación del resto de los elementos para usar bootstrap.

    Pocas ideas hay pues desde que partimos hemos logrado casi lo que queríamos, tal vez la parte del los bloques con subtitulos con imágenens desplegables refactorizarlos completamente en presentar pestañas a elegir secciones de navegación pero tal vez ver la forma de mantener la visualización permanente de las mismas.

    El footer pocas ideas tenemos de refactorizarlo.

5/agosto/2022
    Decidimos incluir un carrusel con control refactorizandolo con 4 bloques correspondientes al mismo orden de subtitulos que usabamos por cada bloque de imágenes, o sea que reemplazamos el scroll de cadar artículo con sus correspondientes subtitulos h2 con un carrusel dinámico

    Las imágenes logramos conservar la animación 3D de siempre y el detalle a solucionar tal vez al final antes de la entrega es ajustar el borde cuadrado de las cards 3D a el original borde redondeado, aparentemente alguna clase de Bootstrap está interfiriendo.

    Si en las pruebas logramos a este carrusel incluirlo en una barra de navegación para cada articulo de los h2 necesitaremos crear 4 secciones mas de cada una sin carrusel o bien pasar a una sóla página con una columna sin transiciones colapsables muy similar a lo que veniamos realizando antes.

    El borde de las cards lo pudimos reestablecer bien, sucedía que una de las clases de los <div> era class= "card" que coincidía con una de las usadas en bootstrap. La cambiamos por cardBlock y solucionado.
    
7/agosto/2022
    Implementamos bootstrap a la página de explore.html con un elemento de bootstrap Scroolspy Navbar insertando los bloques de las mismas imagenes en cards 3D para generar un scroll de imánenes ordenados para moverse un poco más comodo por búsqueda sencilla e intuitiva.

    En principio al pegar el código del navbar scrollspy sucedía que la misma barra quedaba oculta debajo de la otra primer barra de navegación, se hicieron intentos de utilizar z-index para generar un scroll con tope superior antes de la primer barra de navegación, sin éxito.

    Entonces en consultar y ver documentación nos topamos con la ayuda de un articulo ( https://www.campusmvp.es/recursos/post/como-usar-position-sticky.aspx ) para implementar sticky , hemos copiado el código de estilo tal cual lo utilizó esta persona y funcionó muy bien sin hacer ajustes.

    #navbar-example2 {
    position: sticky;
    top:80px;
    border:10px solid #1B75CE;
    display:inline-block;
}

    con esto fijamos la barra hasta antes de tocar la primer barra.

    Insertamos todas las cards 3D en reemplazo de los textos y no funcionaba el hover de cada una.

    Fue por motivo de coincidencia de una clase que usa bootstrap, la clase .card , la modificamos en los estilos por .cardBlock pero no lo hicimos en el hover. Corregido ya funcionaba todo de maravilla.

    Armando la barra de navegación de las imágenes en explre.html existen dificultades en que tenga buen funcionamiento con redireccionar bien los hipervinculos con los botones de navegación, los pone a los subtitulos fuera de lugar, generalmente mas arriba donde no se ven.

    Intentamos implementar overflow-y: scroll con 
    .main__scroll{
    min-height: 90vh;
    max-height: 90vh;
    overflow-y: scroll;
}
.main__scroll::-webkit-scrollbar{
    width:0px;
}
    y los controles se comportan muy mal, si se arregla la continuidad entre footer, video background y header, sin espacios en blanco.

    Corregimos la mayoría de los errores, funciona medianamente bien, falta ocuparnos del responsive y algunos detalles mas tal vez.


