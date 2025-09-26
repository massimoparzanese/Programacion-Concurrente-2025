## Consigna
Existe una comisión de 50 alumnos que deben realizar tareas de a pares, las cuales son
corregidas por un JTP. Cuando los alumnos llegan, forman una fila. Una vez que están todos
en fila, el JTP les asigna un número de grupo a cada uno. Para ello, suponga que existe una
función AsignarNroGrupo() que retorna un número “aleatorio” del 1 al 25. Cuando un alumno
ha recibido su número de grupo, comienza a realizar su tarea. Al terminarla, el alumno le avisa
al JTP y espera por su nota. Cuando los dos alumnos del grupo completaron la tarea, el JTP
les asigna un puntaje (el primer grupo en terminar tendrá como nota 25, el segundo 24, y así
sucesivamente hasta el último que tendrá nota 1). Nota: el JTP no guarda el número de grupo
que le asigna a cada alumno.

> Acomodar y formar barrera con los alumnos en la fila, que el profesor solo se encargue del escritorio. Que los alumnos se coordinen entre ellos
## Resolución

```
Monitor Fila {
    cond [50] fila;
    cond condJ;
    int esperando = 0;
    int [50] nroGrupo ;
    procedure formarFila(idA: in int, numG: out int){
        esperando ++;
        if(esperando == 50) signal(condJ)
        wait(fila[idA])
        numG = nroGrupo[idA]
    }
    procedure asignarTareas(){
        if(esperando < 50) wait(condJ);
        for i 0 to 49{
            int num;
            asignadorTareas.asignar(num)
            nroGrupo[i] = num;
            signal(fila[i])
        }
    }
}
Monitor asignadorTareas { // puede que esto este de mas y se pueda hacer en la misma fila
    procedure asignar(num: out int){
        num = AsignarNroGrupo()
    }
}
Monitor Escritorio {
    cond [25] filaNotas;
    cond avisar;
    int nota = 25;
    int gruposEsperando = 0;
    int [25] grupos;
    int [25] notas;
    cola grupo;
    procedure dejarTarea(idG: in int, puntaje: out int){
        grupos[idG] ++;
        if(grupos[idG] == 2){
            gruposEsperando ++;
            push(cola,idG)
            signal(avisar)
        }
        wait(filaNotas[idG])
        puntaje = notas[idG]
    }
    procedure calificar(){
        if(gruposEsperando == 0) wait(avisar)
        int idAux;
        idAux = pop(cola);
        gruposEsperando --;
        notas[idAux] = nota;
        nota --;
        signal_all(filaNotas[idAux])
    }
}
process alumno[id: 0..49]{
    int numG;
    int puntaje;
    Fila.formarFila(id,numG)
    \\ realiza tarea
    Escritorio.dejarTarea(numG,puntaje)
    
}
process jtp{
    Fila.asignarTareas()
    for i 1 to 25{
        Escritorio.calificar();
    }

}
```