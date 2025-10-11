## Consigna

Suponga que N clientes llegan a la cola de un banco y que serán atendidos por sus
empleados. Analice el problema y defina qué procesos, recursos y canales/comunicaciones
serán necesarios/convenientes para resolverlo. Luego, resuelva considerando las siguientes
situaciones:
    a. Existe un único empleado, el cual atiende por orden de llegada.
    b. Ídem a) pero considerando que hay 2 empleados para atender, ¿qué debe
    modificarse en la solución anterior?
    c. Ídem b) pero considerando que, si no hay clientes para atender, los empleados
    realizan tareas administrativas durante 15 minutos. ¿Se puede resolver sin usar
    procesos adicionales? ¿Qué consecuencias implicaría?


## Resolución


### Inciso A


```
chan pedidos(int); // pedidos a empleado por orden de llegada
chan pasar[0..N-1](bool); // canal para esperar a ser atendido
process Cliente [id:0..N-1]{
    bool ok;
    send pedidos(id)
    recieve pasar[id](ok); // Espera a ser atendido
}
process Empleado {
    int idP = -1;
    for i = 1 to N{
        recieve pedidos(idP);
        send pasar[idP](true); // avisa para pasar
        // Atiende a la persona
    }
}
```

### Inciso B

```
chan pedidos(int); // pedidos a empleado por orden de llegada
chan pasar[0..N-1](bool); // canal para esperar a ser atendido
process Cliente [id:0..N-1]{
    bool ok;
    send pedidos(id)
    recieve pasar[id](ok); // Espera a ser atendido
}
process Empleado[id:0..1] {
    int idP = -1;
    for i = 1 to N{
        recieve pedidos(idP);
        send pasar[idP](true); // avisa para pasar
        // Atiende a la persona
    }
}
```


### Inciso C

```
chan pedidos(int); // pedidos a empleado por orden de llegada
chan enEspera(int)
chan siguiente[2](int);
chan pasar[0..N-1](bool); // canal para esperar a ser atendido
process Cliente [id:0..N-1]{
    bool ok;
    send pedidos(id)
    recieve pasar[id](ok); // Espera a ser atendido
}
process Empleado[id:0..1] {
    int idP = -1;
    while(true){
        send enEspera(id)
        recieve siguiente[id](idP)
        if(idP <> -1) {
            send pasar[idP](true); // avisa para pasar
            // Atiende a la persona
        } 
        else delay(15 minutos); // realiza tareas administrativas   
    }
}
process Coordinador {
    int idEmple;
    int idP;
    for i 1 to N {
        if(!empty(pedidos)) recieve pedidos(idP)
        else idP = -1;
        recieve enEspera(idEmple)
        send siguiente[idEmple](idP)
    }
}
```
