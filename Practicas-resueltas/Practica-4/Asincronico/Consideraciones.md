## CONSIDERACIONES PARA RESOLVER LOS EJERCICIOS DE PASAJE DE
### MENSAJES ASINCRÓNICO (PMA):
- Los canales son compartidos por todos los procesos.
- Cada canal es una cola de mensajes; por lo tanto, el primer mensaje encolado es el primero en ser atendido.
- Por ser PMA, el send no bloquea al emisor.
- Se puede usar la sentencia empty para saber si hay algún mensaje en el canal, pero no se puede consultar por la cantidad de mensajes encolados.
- Se puede utilizar el if/do no determinístico donde cada opción es una condición boolena donde se puede preguntar por variables locales y/o por empty de canales.
    if (cond 1) -> Acciones 1;
    (cond 2) -> Acciones 2;
    ….
    (cond N) -> Acciones N;
    end if
    De todas las opciones cuya condición sea Verdadera elige una en forma no determinística y 
    ejecuta las acciones correspondientes. Si ninguna es verdadera, sale del if/do sin ejecutar acción alguna.
-  Se debe evitar hacer busy waiting siempre que sea posible (sólo hacerlo si no hay otra opción).
- En todos los ejercicios el tiempo debe representarse con la función delay