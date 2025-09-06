## Consigna

En una empresa de logística de paquetes existe una sala de contenedores donde se
preparan las entregas. Cada contenedor puede almacenar un paquete y la sala cuenta con
capacidad para N contenedores. Resuelva considerando las siguientes situaciones:
a) La empresa cuenta con 2 empleados: un empleado Preparador que se ocupa de
preparar los paquetes y dejarlos en los contenedores; un empleado Entregador
que se ocupa de tomar los paquetes de los contenedores y realizar la entregas.
Tanto el Preparador como el Entregador trabajan de a un paquete por vez.
b) Modifique la solución a) para el caso en que haya P empleados Preparadores.
c) Modifique la solución a) para el caso en que haya E empleados Entregadores.
d) Modifique la solución a) para el caso en que haya P empleados Preparadores y E
empleadores Entregadores.

## Resolución

### A
```
sem contenedor = N;
sem lleno = 0;
int ocupado = 0; libre = 0;
recurso contenedores[N];
process preparador{
    while (true){
        // produce datos
        P(contenedor)
        contenedores[libre] = datos;
        libre = (libre + 1) mod N;
        V(lleno) 
    }
}
process consumidor{
    resultado res;
    while (true){
        // produce datos
        P(lleno)
        res = contenedores[ocupado]
        ocupado = (ocupado + 1) mod N
        V(contenedor) 
        // utiliza el resultado
    }
}
```
### B
```
sem contenedor = N;
sem lleno = 0;
sem mutexPreparador = 1;
int ocupado = 0; libre = 0;
recurso contenedores[N];
process preparador[id: 0...N-1]{
    while (true){
        // produce datos
        P(contenedor)
        P(mutexPreparador)
        contenedores[libre] = datos;
        libre = (libre + 1) mod N;
        V(mutexPreparador)
        V(lleno) 
    }
}
process consumidor{
    resultado res;
    while (true){
        // produce datos
        P(lleno)
        res = contenedores[ocupado]
        ocupado = (ocupado + 1) mod N
        V(contenedor) 
        // utiliza el resultado
    }
}
```

### C
```
sem contenedor = N;
sem lleno = 0;
sem mutexConsumidor = 1;
int ocupado = 0; libre = 0;
recurso contenedores[N];
process preparador{
    while (true){
        // produce datos
        P(contenedor)
        contenedores[libre] = datos;
        libre = (libre + 1) mod N;
        V(lleno) 
    }
}
process consumidor[id: 0...N-1]{
    resultado res;
    while (true){
        // produce datos
        P(lleno)
        P(mutexConsumidor)
        res = contenedores[ocupado]
        ocupado = (ocupado + 1) mod N
        V(mutexConsumidor)
        V(contenedor) 
        // utiliza el resultado
    }
}
```

### D

```
sem contenedor = N;
sem lleno = 0;
sem mutexPreparador = 1;
sem mutexConsumidor = 1;
int ocupado = 0; libre = 0;
recurso contenedores[N];
process preparador[id: 0...N-1]{
    while (true){
        // produce datos
        P(contenedor)
        P(mutexPreparador)
        contenedores[libre] = datos;
        libre = (libre + 1) mod N;
        V(mutexPreparador)
        V(lleno) 
    }
}
process consumidor[id: 0...N-1]{
    resultado res;
    while (true){
        // produce datos
        P(lleno)
        P(mutexConsumidor)
        res = contenedores[ocupado]
        ocupado = (ocupado + 1) mod N
        V(mutexConsumidor)
        V(contenedor) 
        // utiliza el resultado
    }
}
```