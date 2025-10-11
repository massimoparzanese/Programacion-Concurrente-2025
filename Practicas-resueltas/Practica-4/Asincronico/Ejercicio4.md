## Consigna

Simular la atención en un locutorio con 10 cabinas telefónicas, el cual tiene un empleado
que se encarga de atender a N clientes. Al llegar, cada cliente espera hasta que el empleado
le indique a qué cabina ir, la usa y luego se dirige al empleado para pagarle. El empleado
atiende a los clientes en el orden en que hacen los pedidos. A cada cliente se le entrega un
ticket factura por la operación.
a) Implemente una solución para el problema descrito.
b) Modifique la solución implementada para que el empleado dé prioridad a los que
terminaron de usar la cabina sobre los que están esperando para usarla.
Nota : maximizar la concurrencia; suponga que hay una función Cobrar() llamada por el empleado que simula que el empleado le cobra al cliente


## Resolución 

### Inciso A

```
chan pedidos(int)
chan asignada[0..N-1](int)
chan pagar(int,text);
chan cabinasLibres(int) // Supongo que ya tiene todas las cabinas cargadas 
chan ticket[0..N-1](text)
process empleado {
    int idP;
    text pago;
    int terminal;
    while(true){
        if(!empty(cabinasLibres) && !empty(pedidos)){
            recieve pedidos(idP)
            recieve cabinasLibres(terminal)
            send asignada[idP](terminal)
        }
        if(!empty(pagar)){
            recieve pagar(idP,pago)
            send ticket[idP](Cobrar(pago))
        }
    }
}
process persona[id:0..N-1]{
    int terminal;
    text pago;
    text ticketPago;
    send pedidos(id)
    recieve asignada[id](terminal)
    // uso la terminal
    send cabinasLibres(terminal)
    send pagar(id,pago)
    recieve ticket[id](ticketPago)
}
```



### Inciso B

```
chan pedidos(int)
chan asignada[0..N-1](int)
chan pagar(int,text);
chan cabinasLibres(int) // Supongo que ya tiene todas las cabinas cargadas 
chan ticket[0..N-1](text)
process empleado {
    int idP;
    text pago;
    int terminal;
    while(true){
        if(!empty(pagar)){
            recieve pagar(idP,pago)
            send ticket[idP](Cobrar(pago))
        }
        if(!empty(cabinasLibres) && !empty(pedidos)){
            recieve pedidos(idP)
            recieve cabinasLibres(terminal)
            send asignada[idP](terminal)
        }

    }
}
process persona[id:0..N-1]{
    int terminal;
    text pago;
    text ticketPago;
    send pedidos(id)
    recieve asignada[id](terminal)
    // uso la terminal
    send cabinasLibres(terminal)
    send pagar(id,pago)
    recieve ticket[id](ticketPago)
}
```

