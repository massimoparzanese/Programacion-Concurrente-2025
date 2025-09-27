## Consigna 
Se debe simular una maratón con C corredores donde en la llegada hay UNA máquina
expendedoras de agua con capacidad para 20 botellas. Además, existe un repositor encargado
de reponer las botellas de la máquina. Cuando los C corredores han llegado al inicio comienza
la carrera. Cuando un corredor termina la carrera se dirigen a la máquina expendedora, espera
su turno (respetando el orden de llegada), saca una botella y se retira. Si encuentra la máquina
sin botellas, le avisa al repositor para que cargue nuevamente la máquina con 20 botellas;
espera a que se haga la recarga; saca una botella y se retira. Nota: mientras se reponen las
botellas se debe permitir que otros corredores se encolen.

## Resolucion

```
Monitor Maraton{
    cond espera;
    esperando = 0;
    procedure esperarSalida(){
        esperando ++;
        if(esperando == 50) signal_all(espera)
    }
}
Monitor Maquina{
    cond[50] esperarBotellas;
    cola colaP;
    int cant = 20;
    cond repo;
    boolean libre = true;
    procedure consumir(idP: in int){
        if(!libre){
            push(cola, idP)
            wait(esperarBotellas[idP])
        }
        else if(cant == 0){
            signal(repo)
            libre = false
            push(cola, idP)
            wait(esperarBotellas[idP])
        }
        else libre = false;
        cant--;
        if(!empty(colaP)){
            int persona = pop(colaP)
            signal(esperarBotellas[persona])
        }
        else libre = true;
    }

    procedure esperarAviso(){
        if(cant > 0) wait(repo)
    }
    procedure reponer(){
        cant = 20;
        int idP = pop(colaP)
        signal(esperarBotellas[idP])
    }
}
process corredor[id:0..C-1]{
    Maraton.esperarSalida()
    // Corre maraton
    Maquina.consumir(id)
}
process repositor{
    while(true){
        Maquina.esperarAviso()
        // repone botellas
        Maquina.reponer()
    }
}
```