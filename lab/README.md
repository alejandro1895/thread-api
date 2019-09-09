# Thread API # 

En esta sección escribiremos algunos programas multi-hilo y usaremos una herramienta específica llamada ```helgrind``` para encontrar problemas en estos programas. 

## Questions ##

1. Primero codifique ```main-race.c```. Examine el código de manera que usted pueda ver (ojalá de manera obvia) un data race en el código. Ahora ejecute ```helgrind``` (al teclear ```valgrind --tool=helgrind ./main-race```) y vea como este programa reporta los *data races*. ¿Se muestran las líneas de código involucradas?, ¿Qué otra información entrega este programa?

En el código se puede observar que existe un problema de sincronización, y que se tiene un recurso compartido (variable global balance) entre hilos que al no contar con un mecanismo de bloqueo, nos origina una condición de carrera.

<<Imagen1.1>
Al ejecutar el código con helgrind, efectivamente nos indica que se detectaron errores de sincronización y nos muestra las direcciones de memoria de las instrucciones en donde se lograron identificar los problemas.
<<Imagen1.2>
2. ¿Qué ocurre cuando usted elimina una de las líneas que generan problemas en el código? Ahora agrege un lock alrededor de las actualizaciones de la variable compartida, y entonces alrededor de ambas. ¿Qué reporta ```helgrind``` en cada uno de estos casos?
Si comentamos la línea de código del hilo hijo en donde disminuye en 1 la variable compartida.
<Imagen2.1>
Puede observarse que no se detecta ningún error de sincronización, lo cual tiene sentido ya que solo el hilo padre estaría accediendo a la variable.
<Imagen2.2>
Al agregar un lock alrededor de las actualizaciones de la variable compartida balance, vemos que nuevamente Helgrind no detecta ningún error de sincronización, ya que con este mecanismo imponemos límites al acceso del recurso en un entorno donde hay varios hilos en ejecución . Este bloqueo está diseñado para hacer cumplir una política de control de concurrencia de exclusión mutua.
<Imagen2.3>
3. Ahora observe ```main-deadlock.c```. Examine el código. Este código tiene un problema conocido como deadlock. ¿Puede ver que problema podrá este tener?
4. Ahora ejecute ```helgrind``` en este código. ¿Qué reporta helgrind?
5. Ahora ejecute ```helgrind``` en ```main-deadlock-global.c```. Examine el código. ¿Tiene este el mismo problema que ```main-deadlock.c```? ¿Muestra ```helgrind``` el mismo reporte de error? ¿Qué dice esto a cerca de herramientas como ```helgrind```?
6. Ahora observe ```main-signal.c```. Este código usa una variable (```done```) para señalar que el hijo esta hecho y que el padre puede continuar. ¿Por qué este códido es ineficiente? (En que termina el padre dedicando su tiempo, si el hijo toma una gran cantidad de tiempo en completarse).
7. Ahora ejecute ```helgrind``` para este programa. ¿Qué reporta helgrind?, ¿Es correcto el código?
8. Ahora observe una versión levemente modificada del código, la cual es encontrada en ```main-signal-cv.c```. Esta versión usa una variable de condición para señalizar (y asociar un lock). ¿Por qué este código es mejor que la versión previa? ¿Es la corrección, o el desempeño, o ambos?
9. Ejecute de nuevo ```helgrind``` en ```main-signal-cv``` ¿Reporta algunos errores?

**Nota**: Se adjuntan los códigos para facilitar en análisis.
