## Consigna

Implemente una solución para el siguiente problema. Se debe simular el uso de una máquina
expendedora de gaseosas con capacidad para 100 latas por parte de U usuarios. Además, existe un
repositor encargado de reponer las latas de la máquina. Los usuarios usan la máquina según el orden
de llegada. Cuando les toca usarla, sacan una lata y luego se retiran. En el caso de que la máquina se
quede sin latas, entonces le debe avisar al repositor para que cargue nuevamente la máquina en forma
completa. Luego de la recarga, saca una botella y se retira. Nota: maximizar la concurrencia; mientras
se reponen las latas se debe permitir que otros usuarios puedan agregarse a la fila.

## Resolucion

```
sem mutexCola = 1;
Cola c;
sem espera[U] = ([U] 0);
sem repo = 0;
bool libre = true;
int latas = 100;
sem esperaAviso = 0;
process usuario[id: 0..U-1]{
    P(mutexCola)
    if(libre){
        libre = false;
        V(mutexCola)
    }
    else {
        push(c,id)
        V(mutexCola)
        P(espera[id])
    }
    if(latas == 0){
        V(repo)
        P(esperaAviso)
    }
    latas --; // consultar si puede haber riesgo de dos procesos (persona y repositor) modificando latas segun condicion de carrera
    P(mutexCola)
    if(c.isEmpty()){
        libre = true;
    }
    else {
        int sig = pop(c);
        V(espera[sig])
    }
    V(mutexCola)
}
process repositor{
    P(repo)
    // repone latas
    latas = 100;
    V(esperaAviso)
}
```
