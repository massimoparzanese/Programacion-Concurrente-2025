## Consigna

A una cerealera van T camiones a descargarse trigo y M camiones a descargar maíz. Sólo
hay lugar para que 7 camiones a la vez descarguen, pero no pueden ser más de 5 del mismo
tipo de cereal.
a) Implemente una solución que use un proceso extra que actúe como coordinador
entre los camiones. El coordinador debe atender a los camiones según el orden de
llegada. Además, debe retirarse cuando todos los camiones han descargado.
b) Implemente una solución que no use procesos adicionales (sólo camiones). No
importa el orden de llegada para descargar. Nota: maximice la concurrencia.

## Resolución


### A

>[!Important] Consultar ya que me genera duda que haya demasiados semáforos y el hecho de preguntar por el tipo también se me hace raro
```
sem c_trigo[T] = ([T] 0);
sem c_maiz[M] = ([M] 0);
sem max = 7;
sem trigo = 5;
sem maíz = 5;
sem mutex = 1;
sem mutex_total = 1;
Cola cola;
process camion_trigo[id:0...T-1]{
        P(mutex)
        push(cola,(id,trigo))
        V(mutex)
        V(llegaron)
        P(c_trigo[id])
        // descarga
        V(trigo)
        V(max)
        P(mutex_total)
        total = total + 1;
        V(mutex_total)
}
process camion_maiz[id:0...M-1]{
    P(mutex)
    push(cola,(id,maiz))
    V(mutex)
    V(llegaron)
    P(c_trigo[id])        
    // descarga
    V(trigo)
    V(max)
    P(mutex_total)
    total = total + 1;
    V(mutex_total)
}
process coordinador{
    int total = 0;
    String tipo;
    int id;
    P(mutex_total)
    while(total < (T+M)){
        V(mutex_total)
        P(llegaron)
        P(mutex)
        id,tipo = pop(cola)
        V(mutex)
        if(tipo == trigo){
            P(trigo)
            P(max)
            V(c_trigo[id])
        }
        else if(tipo == maiz){
            P(maiz)
            P(max)
            V(c_trigo[id])
        }
        P(mutex_total)
    }
}
```

### B
```
sem max = 7;
sem trigo = 5;
sem maíz = 5;
sem mutex = 1;
Cola cola;
process camion_trigo[id:0...T-1]{
       // llega camión
       P(trigo)
       P(max)
       // descarga
       V(max)
       V(trigo)
}
process camion_maiz[id:0...M-1]{
    // llega camión
       P(maiz)
       P(max)
       // descarga
       V(max)
       V(maiz)
}
```