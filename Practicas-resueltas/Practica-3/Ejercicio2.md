## Consigna

Existen N procesos que deben leer información de una base de datos, la cual es administrada
por un motor que admite una cantidad limitada de consultas simultáneas.
a) Analice el problema y defina qué procesos, recursos y monitores/sincronizaciones
serán necesarios/convenientes para resolverlo.
b) Implemente el acceso a la base por parte de los procesos, sabiendo que el motor de
base de datos puede atender a lo sumo 5 consultas de lectura simultáneas.


## Resolución


### Inciso A

Tendré N procesos los cuales interactúan con un monitor que será el motor de BD el cual tendrá un proceso que será solicitarLectura y otro que sea finalizarLectura. Habrá una variable condicion que funcionará como una especie de cola en caso de que ya estén realizando 5 consultas en simultáneo


### Inciso B


```
Monitor motor_bd {
    cond cola;
    int cant = 0;
    
    procedure solicitarLectura(){
        while(cant == 5){
            wait(cola);
        }
        cant = cant + 1;
    }
    
    procedure finalizarLectura(){
        cant = cant - 1;
        signal(cola);
    }
}
process persona[id:0..N-1]{
    motor_bd.solicitarLectura();
    // lee de la bd
    motor_bd.finalizarLectura();
}
```