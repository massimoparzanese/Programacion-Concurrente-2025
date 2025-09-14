## Consigna
6. Existen N personas que deben imprimir un trabajo cada una. Resolver cada ítem usando
semáforos:
a) Implemente una solución suponiendo que existe una única impresora compartida por
todas las personas, y las mismas la deben usar de a una persona a la vez, sin importar
el orden. Existe una función Imprimir(documento) llamada por la persona que simula el
uso de la impresora. Sólo se deben usar los procesos que representan a las Personas.
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.
c) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el
orden dado por el identificador del proceso (la persona X no puede usar la impresora
hasta que no haya terminado de usarla la persona X-1).
d) Modifique la solución de (b) para el caso en que además hay un proceso Coordinador
que le indica a cada persona que es su turno de usar la impresora.
e) Modificar la solución (d) para el caso en que sean 5 impresoras. El coordinador le
indica a la persona cuando puede usar una impresora, y cual debe usar.

## Resolución

### A
```
sem mutex = 1;
process persona[id:0..N-1]{
    Documento doc;
    P(mutex)
    Imprimir(doc);
    V(mutex)
}
```

### B
```
sem mutex = 1;
sem espera[N] = ([N] 0)
Cola c;
int proximo;
bool libre = true;
process persona[id:0..N-1]{
    Documento doc;
    P(mutex)
    if(!empty(c)){
        push(c,id)
        V(mutex)
    }
    if(libre){
        libre = false
        V(mutex)
    }
    else {
        push(c, id);     // me encolo
        V(mutex);
        P(espera[id]);   // espero a que me despierten
    }

    Imprimir(doc);

     P(mutex);
    if (empty(c)) {
        libre = true;    // dejo libre la impresora
    } else {
        proximo = pop(C);
        V(espera[sig]);  // despierto al siguiente en la cola
    }
    V(mutex);
}
```

### C
```
sem espera[N] = ([N] 0)
Cola c;
process persona[id:0..N-1]{
    Documento doc;
    if(id > 0){
        P(espera[id])
    }
    Imprimir(doc);
    if(id < N-1){
        V(espera[id+1])
    }
}
```

### D
```
sem mutex = 1;
sem pedidos = 0;
sem espera[N] = ([N] 0)
sem libre = 0;
Cola c;
int proximo;
process persona(id:0...N){
    Documento doc;
    while(true){ // puede no estar
        P(mutex)
        push(c,id)
        V(mutex)
        V(pedidos)
        P(espera[id])
        Imprimir(doc)
        V(libre)
    }
}
process coordinador{
    while(true){ // podría ser hasta N
        P(pedidos)
        P(mutex)
        proximo = pop(c);
        V(mutex)
        V(espera[proximo])
        P(libre)
    }

}
```