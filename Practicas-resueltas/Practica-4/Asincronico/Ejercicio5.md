## Consigna

Resolver la administración de 3 impresoras de una oficina. Las impresoras son usadas por N
administrativos, los cuales están continuamente trabajando y cada tanto envían documentos
a imprimir. Cada impresora, cuando está libre, toma un documento y lo imprime, de
acuerdo con el orden de llegada.
a) Implemente una solución para el problema descrito.
b) Modifique la solución implementada para que considere la presencia de un director de
oficina que también usa las impresas, el cual tiene prioridad sobre los administrativos.
c) Modifique la solución (a) considerando que cada administrativo imprime 10 trabajos y
que todos los procesos deben terminar su ejecución.
d) Modifique la solución (b) considerando que tanto el director como cada administrativo
imprimen 10 trabajos y que todos los procesos deben terminar su ejecución.
e) Si la solución al ítem d) implica realizar Busy Waiting, modifíquela para evitarlo.
Nota: ni los administrativos ni el director deben esperar a que se imprima el documento.

## Resolucion

### Inciso A

```
chan pedidos(int,doc)
chan devoluciones[0..N-1](doc)
process Impresora[id:0..2]{
    int idP;
    doc documento;
    doc devolucion;
    while(true){
        recieve pedidos(idP,docomento)
        devolucion = Imprimir(documento)
        send devoluciones[idP](devolucion)
    }
}
process administrativo[id:0..N-1]{
    doc documento;
    doc devolucion;
    while (true){
        documento = ...; // genera nuevo documento
        send pedidos(id,documento);
        recieve devoluciones[id](devolucion)
        // Después de recibir la devolución, genera el siguiente documento en el loop.
    }
}

```



### Inciso B

```
chan pedidos(int,doc)
chan prioritarios(int, doc)
chan devoluciones[0..N](doc)
process Impresora[id:0..2]{
    int idP;
    doc documento;
    doc devolucion;
    bool encontre;
    while(true){
        encontre = false;
        if (!empty(prioritarios)){
            recieve prioritarios(idP,docomento)
            encontre = true;
        }
        else if(!empty(pedidos)){
            recieve pedidos(idP,docomento)
            encontre = true;
        }

        if(encontre){
            devolucion = Imprimir(documento)
            send devoluciones[idP](devolucion)
        }
        
    }
}
process administrativo[id:0..N-1]{
    doc documento;
    doc devolucion;
    while (true){
        documento = ...; // genera nuevo documento
        send pedidos(id,documento);
        recieve devoluciones[id](devolucion)
        // Después de recibir la devolución, genera el siguiente documento en el loop.
    }
}
process director {
    doc documento;
    doc devolucion;
    while (true){
        documento = ...; // genera nuevo documento
        send prioritarios(N,documento);
        recieve devoluciones[N](devolucion)
        // Después de recibir la devolución, genera el siguiente documento en el loop.
    }
}
```


### Inciso C

### Inciso D


### Inciso E