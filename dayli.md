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

