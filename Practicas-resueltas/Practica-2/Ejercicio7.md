## Consigna
Suponga que se tiene un curso con 50 alumnos. Cada alumno debe realizar una tarea y
existen 10 enunciados posibles. Una vez que todos los alumnos eligieron su tarea,
comienzan a realizarla. Cada vez que un alumno termina su tarea, le avisa al profesor y se
queda esperando el puntaje del grupo (depende de todos aquellos que comparten el
mismo enunciado). Cuando un grupo terminó, el profesor les otorga un puntaje que
representa el orden en que se terminó esa tarea de las 10 posibles.
Nota: Para elegir la tarea suponga que existe una función elegir que le asigna una tarea a
un alumno (esta función asignará 10 tareas diferentes entre 50 alumnos, es decir, que 5
alumnos tendrán la tarea 1, otros 5 la tarea 2 y así sucesivamente para las 10 tareas).

## Resolución
```
int contrador_grupo[10] = ([10] 0)
sem barrera[10] = ([10] 0)
int puntajes[10] = ([10] 0)
sem mutex = 1;
sem avisar_profesor = 0;
cola avisos;
process alumno[id: 0...49]{
    int tarea = elegir();
    // realiza su tarea

    // Aviso al profesor
    P(mutex);
    contador[tarea] = contador[tarea] + 1;
    push(avisos,tarea)
    V(mutex);
    V(avisar_profesor)

    // Quedo esperando mi puntaje
    P(barrera[tarea]);
}

process profesor{
    int puntaje = 1;
     while (true) {
        P(avisar_profesor)
        P(mutex)
        int tarea = pop(avisos)
        if(contador[tarea] == 5){
            puntajes[tarea] = puntaje;
            puntaje = puntaje + 1;
            for i:1 to 5{
                V(barrera[tarea]);
            }
        }
        V(mutex)
     }
}
```