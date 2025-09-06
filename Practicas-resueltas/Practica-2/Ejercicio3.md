## Consigna

Un sistema operativo mantiene 5 instancias de un recurso almacenadas en una cola.
Además, existen P procesos que necesitan usar una instancia del recurso. Para eso, deben
sacar la instancia de la cola antes de usarla. Una vez usada, la instancia debe ser encolada
nuevamente para su reúso.

## Resolución

sem mutex = 1, esperar_recursos = 5;
cola c;
process consumidor[id:0..P-1]{
    recurso rec;
    while (true){
        P(esperar_recursos) // indica que va a sacar un recurso de la cola
        P(mutex) // exclusión mutua para utilizar la cola
        rec = pop(c);
        V(mutex) // desocupa el recurso
        // usa el recurso
        P(mutex)
        push(c,rec);
        V(mutex)
        V(esperar_recursos) // indica que el recurso volvió a la cola
    }
}