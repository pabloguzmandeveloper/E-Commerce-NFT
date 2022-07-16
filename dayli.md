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


