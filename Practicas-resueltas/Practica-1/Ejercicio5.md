## Preguntas
---
5. En cada ítem debe realizar una solución concurrente de grano grueso (utilizando <> y/o
<await B; S>) para el siguiente problema, teniendo en cuenta las condiciones indicadas en el
item. Existen N personas que deben imprimir un trabajo cada una.
    a) Implemente una solución suponiendo que existe una única impresora compartida por
    todas las personas, y las mismas la deben usar de a una persona a la vez, sin importar el
    orden. Existe una función Imprimir(documento) llamada por la persona que simula el uso
    de la impresora. Sólo se deben usar los procesos que representan a las Personas.
    b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.
    c) Modifique la solución de (a) para el caso en que se deba respetar el orden dado por el
    identificador del proceso (cuando está libre la impresora, de los procesos que han
    solicitado su uso la debe usar el que tenga menor identificador).
    d) Modifique la solución de (b) para el caso en que además hay un proceso Coordinador que
    le indica a cada persona que es su turno de usar la impresora.

## Respuestas
---
### Item A

```
process Persona(i:0..N-1){
    Documento doc;
    <Imprimir(doc)>
}
```

### Item B
```
colaEspecial cola;
int Siguiente = -1;
process Persona(i:0..N-1){
    Documento doc;
    <
    if(Siguiente == -1){
        Siguiente = i;
    }
    else{
        push(cola,i)
    }
    >
    <await (Siguiente == i);>
    Imprimir(doc)
    <if (empty(cola)) Siguiente = -1;
    else Siguiente = pop(cola)>;
}
```
### Item C
```
```

### Item D
```
colaEspecial cola;
int Siguiente = -1;
bool termine = false;
process Persona(i:0..N-1){
    Documento doc;
    <push(cola,i)>
    <await (Siguiente == i);>
    Imprimir(doc)
    termine = true;
    
}
process Cordinador::{
    while(true){
        <await (!empty(cola));>         
        <Siguiente = pop(cola);
         termine = false;>;             
        <await (termine);>;     
    }
}
```