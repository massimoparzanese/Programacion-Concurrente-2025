## Preguntas
--- 
4. Resolver con SENTENCIAS AWAIT (<> y <await B; S>). Un sistema operativo mantiene
5 instancias de un recurso almacenadas en una cola, cuando un proceso necesita usar una
instancia del recurso la saca de la cola, la usa y cuando termina de usarla la vuelve a depositar.

## Respuestas
--- 
Supongo a la hora de agregar elementos y sacarlos de una cola, tengo 2 métodos: push(cola,elemento) y pop(cola) que me ayudaran a manipularla. Y que la cola ya se encuentra inicializada previamente con sus 5 instancias de ese recurso, junto con una variable recurso que sera del tipo que es el recurso.

```
Buffer cola;
int cant;
process Consumidor(id:0...N-1){
    while (true) {
        <await (cant > 0);
        recurso = pop(cola)
        cant = cant - 1>
        // realiza operaciones
        <push(cola, recurso)
        cant = cant + 1>
    }
}
```