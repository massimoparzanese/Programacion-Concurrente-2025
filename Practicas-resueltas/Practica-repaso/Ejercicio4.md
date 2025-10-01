## Consigna

Resolver el siguiente problema. En una elección estudiantil, se utiliza una máquina para voto
electrónico. Existen N Personas que votan y una Autoridad de Mesa que les da acceso a la máquina
de acuerdo con el orden de llegada, aunque ancianos y embarazadas tienen prioridad sobre el resto.
La máquina de voto sólo puede ser usada por una persona a la vez. Nota: la función Votar() permite
usar la máquina.

## Resolucion


```
Monitor Admin {
    Cola colaPrio;
    Cola cola;
    cond irse;
    cond espera[N];
    int sig;
    cond esperarGente;
    int esperando = 0;
    procedure llegada(idP: in int; prioritario: in bool){
        if(prioritario){
            push(colaPrio)
        }
        else push(cola)
        esperando ++;
        signal(esperarGente)
        wait(espera[idP])
    }
    procedure avisarPasar(){
        if(esperando == 0) wait(esperarGente)
        if(!colaPrio.empty()){
            sig = pop(colaPrio)
        }
        else if(cola.empty()) sig = pop(colaPrio)
        wait(irse)
    }

    procedure avisar(){
        signal(irse)
    }
}
process Autoridad{
    for i 1 to N {
        Admin.avisarPasar()
    }
}
process Persona[id:0...N-1]{
    bool prioritario = ...; / En caso de ser persona mayor o embarazada sera true
    Admin.llegada(id,prioritario)
    Votar()
    Admin.avisar();
}
```