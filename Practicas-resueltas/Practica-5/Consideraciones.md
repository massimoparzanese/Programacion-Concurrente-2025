1. NO SE PUEDE USAR VARIABLES COMPARTIDAS
2. Declaración de tareas
• Especificación de tareas sin ENTRY’s (nadie le puede hacer llamados).
TASK Nombre;
TASK TYPE Nombre;
• Especificación de tareas con ENTRY’s (le puede hacer llamados). Los entry’s
funcionan de manera semejante los procedimientos: solo pueden recibir o
enviar información por medio de los parámetros del entry. NO RETORNAN
VALORES COMO LAS FUNCIONES
TASK [TYPE] Nombre IS
ENTRY e1;
ENTRY e2 (p1: IN integer; p2: OUT char; p3: IN OUT float);
END Nombre;
• Cuerpo de las tareas.
TASK BODY Nombre IS
Codigo que realiza la Tarea;
END Nombre;
3. Sincronización y comunicación entre tareas
• Entry call para enviar información (o avisar algún evento).
NombreTarea.NombreEntry (parametros);
• Accept para atender un pedido de entry call sin cuerpo (sólo para recibir el
aviso de un evento para sincronización). Lo usual es que no incluya
parámetros, aunque podría tenerlos. En ese caso, son ignorados por no tener
cuerpo presente.
ACCEPT NombreEntry;
ACCEPT NombreEntry (p1: IN integer; p3: IN OUT float);
• Accept para atender un pedido de entry call con cuerpo.
ACCEPT NombreEntry (p1: IN integer; p3: IN OUT float) do
Cuerpo del accept donde se puede acceder a los parámetros p1 y p3.
Fuera del entry estos parámetros no se pueden usar.
END NombreEntry;
• El accept se puede hacer en el cuerpo de la tarea que ha declarado el entry en
su especificación. Los entry call se pueden hacer en cualquier tarea o en el
programa principal.
4. Select para ENTRY CALL.
• Select ...OR DELAY: espera a lo sumo x tiempo a que la tarea correspondiente haga el
accept del entry call realizado. Si pasó el tiempo entonces realiza el código opcional.
SELECT
NombreTarea.NombreEntry(Parametros);
Sentencias;
OR DELAY x
Código opcional;
END SELECT;
• Select ...ELSE: si la tarea correspondiente no puede realizar el accept inmediatamente
(en el momento que el procesador está ejecutando esa línea de código) entonces se
ejecuta el código opcional.
SELECT
NombreTarea.NombreEntry(Parametros);
Sentencias;
ELSE
Código opcional;
END SELECT;
• En los select para entry call sólo puede ponerse un entry call y una única opción (OR
DELAY o ELSE);
5. Select para ACCEPT.
• En los select para los accept puede haber más de una alternativa de accept, pero no
puede haber alternativas de entry call (no se puede mezclar accept con entries). Cada
alternativa de ACCEPT puede ser o no condicional (uso de cláusula WHEN).
SELECT
ACCEPT e1 (parámetros);
Sentencias1;
OR
ACCEPT e2 (parámetros) IS cuerpo; END e2;
OR
WHEN (condición) => ACCEPT e3 (parámetros) IS cuerpo; END e3;
Sentencias3
END SELECT;
Funcionamiento: Se evalúa la condición booleana del WHEN de cada alternativa (si
no lo tiene se considera TRUE). Si todas son FALSAS se sale del select. En otro caso,
de las alternativas cuya condición es verdadera se elige en forma no determinística una
que pueda ejecutarse inmediatamente (es decir que tiene un entry call pendiente). Si
ninguna de ellas se puede ejecutar inmediatamente el select se bloquea hasta que haya
un entry call para alguna alternativa cuya condición sea TRUE.
• Se puede poner una opción OR DELAY o ELSE (no las dos a la vez).
• Dentro de la condición booleana de una alternativa (en el WHEN) se puede preguntar
por la cantidad de entry call pendientes de cualquier entry de la tarea.
NombreEntry’count
• Después de escribir una condición por medio de un WHEN siempre se debe escribir un
accept.
