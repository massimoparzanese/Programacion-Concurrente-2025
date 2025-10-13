## CONSIDERACIONES PARA RESOLVER LOS EJERCICIOS DE PASAJE DE MENSAJES SINCRÓNICO (PMS):
- Los canales son punto a punto y no deben declararse.
- No se puede usar la sentencia empty para saber si hay algún mensaje en un canal.
- Tanto el envío como la recepción de mensajes es bloqueante.
- Sintaxis de las sentencias de envío y recepción:
Envío:
nombreProcesoReceptor!port (datos a enviar)
Recepción: nombreProcesoEmisor?port (datos a recibir)
El port (o etiqueta) puede no ir. Se utiliza para diferenciar los tipos de mensajes
que se podrían comunicarse entre dos procesos.
- En la sentencia de comunicación de recepción se puede usar el comodín * si el origen es un proceso dentro de un arreglo de procesos. Ejemplo: Clientes[*]?port(datos).
- Sintaxis de la Comunicación guardada:
Guarda: (condición booleana); sentencia de recepción → sentencia a realizar
Si no se especifica la condición booleana se considera verdadera (la condición
booleana sólo puede hacer referencia a variables locales al proceso).
Cada guarda tiene tres posibles estados:
Elegible: la condición booleana es verdadera y la sentencia de comunicación
se puede resolver inmediatamente.
No elegible: la condición booleana es falsa.
Bloqueada: la condición booleana es verdadera y la sentencia de comunicación
no se puede resolver inmediatamente.
Sólo se puede usar dentro de un if o un do guardado:
El if funciona de la siguiente manera: de todas las guardas elegibles se
selecciona una en forma no determinística, se realiza la sentencia de
comunicación correspondiente, y luego las acciones asociadas a esa guarda. Si
todas las guardas tienen el estado de no elegibles, se sale sin hacer nada. Si no
hay ninguna guarda elegible, pero algunas están en estado bloqueado, se queda
esperando en el if hasta que alguna se vuelva elegible.
El do funciona de la siguiente manera: sigue iterando de la misma manera que
el if hasta que todas las guardas hasta que todas las guardas sean no elegibles.