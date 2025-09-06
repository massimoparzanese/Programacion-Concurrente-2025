## Consigna

Una fábrica de piezas metálicas debe producir T piezas por día. Para eso, cuenta con E
empleados que se ocupan de producir las piezas de a una por vez. La fábrica empieza a
producir una vez que todos los empleados llegaron. Mientras haya piezas por fabricar, los
empleados tomarán una y la realizarán. Cada empleado puede tardar distinto tiempo en
fabricar una pieza. Al finalizar el día, se debe conocer cual es el empleado que más piezas
fabricó.
a) Implemente una solución asumiendo que T > E.
b) Implemente una solución que contemple cualquier valor de T y E.

## Resolución

### A y B 
```
sem mutex = 1;
sem mutex_max = 1;
sem barrera = 0;
int emple = 0;
int piezas_fabricadas = 0;
int idMax = 0;
int max = 0;
process empleado[id:1..E-1]{
    int cantidad_producida;
    P(mutex)
    if(emple = E-1){
        for i: 1 to E-1  V(barrera)
        V(mutex)
    }
    else {
        V(mutex)
        P(barrera)
    }
    P(mutex)
    while(piezas_fabricadas < T){
        V(mutex)
        // producir pieza
        cantidad_producida = cantidad_producida + 1;
        P(mutex)
        piezas_fabricadas = piezas_fabricadas + 1;
    }
    V(mutex)
    P(mutex_max)
    if(max <= cantidad_producida){
        max = cantidad_producida;
        idMax = id ;
    }
    V(mutex_max)
}
```