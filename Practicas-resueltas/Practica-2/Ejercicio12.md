## Consigna
.Simular la atención en una Terminal de Micros que posee 3 puestos para hisopar a 150
pasajeros. En cada puesto hay una Enfermera que atiende a los pasajeros de acuerdo
con el orden de llegada al mismo. Cuando llega un pasajero se dirige al Recepcionista,
quien le indica qué puesto es el que tiene menos gente esperando. Luego se dirige al puesto
y espera a que la enfermera correspondiente lo llame para hisoparlo. Finalmente, se retira.
a) Implemente una solución considerando los procesos Pasajeros, Enfermera y
Recepcionista
b)Modifique la solución anterior para que sólo haya procesos Pasajeros y Enfermera,
siendo los pasajeros quienes determinan por su cuenta qué puesto tiene menos
personas esperando.
Nota: suponga que existe una función Hisopar() que simula la atención del pasajero por
parte de la enfermera correspondiente.

## Resolución

### A
```
int esperando[3] = ([3] 0) // cantidad de personas por cola
sem mutex[3] = ([3] 0); // semáforo para el acceso a cada pos del arreglo
sem mutex_cola[3] = ([3] 0); // semáforo para el acceso a cada pos de la cola
sem mutex_r = 1;
sem hay_gente = 0;
Cola r;
Cola cola[3]; // No se si esto se puede hacer
int asignacion[150];// a qué puesto va cada pasajero
sem esperarRespuesta[150] = ([150] 0); // semáforo privado por pasajer
process Recepcionista{
    int total = 0;
    while(total < 150){
        P(hay_gente)
        P(mutex_r)
        id = pop(r);
        V(mutex_r)
        P(mutex[0])
        int menor = esperando[0];
        V(mutex[0])
        int indice = 0;
        for i = 1 to 2 {
            P(mutex[i])
            if (esperando[i] < menor) {
                menor = esperando[i];
                V(mutex[i])
                indice = i;
            }
        }
        asignacion[id] = indice;
        V(esperarRespuesta[id]);
        P(mutex[indice])
        esperando[indice] = esperando[indice] + 1
        V(mutex[indice])
        total = total + 1;
    }
}
process persona[id:0..149]{
    P(mutex_r)
    push(r,id)
    V(mutex_r)
    V(hay_gente)
    P(esperarRespuesta[id])
    int asignado = asignacion[id]
    P(mutex_cola[asignado])
    push(cola[asignado],id)
    V(mutex_cola[asignado])
}
```