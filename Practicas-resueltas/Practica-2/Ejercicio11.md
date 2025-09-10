## Consigna
En un vacunatorio hay un empleado de salud para vacunar a 50 personas. El empleado
de salud atiende a las personas de acuerdo con el orden de llegada y de a 5 personas a la
vez. Es decir, que cuando está libre debe esperar a que haya al menos 5 personas
esperando, luego vacuna a las 5 primeras personas, y al terminar las deja ir para esperar
por otras 5. Cuando ha atendido a las 50 personas el empleado de salud se retira. Nota:
todos los procesos deben terminar su ejecución; suponga que el empleado tienen una
función VacunarPersona() que simula que el empleado está vacunando a UNA persona.

## Resolución

```
sem mutex = 1;
sem barrera = 0;
sem esperar[50] = ([50] 0);
Cola c;
process empleado{
    int total = 0;
    while(total < 50){
        int id;
        for i = 1 to 5: P(barrera)
        for i = 1 to 5:{
            P(mutex)
            id = pop(c)
            V(mutex)
            VacunarPersona()
            V(esperar[id])
            total = total + 1;
        }
    }
}
process persona[id:0..49]{
    P(mutex)
    push(c,id)
    V(mutex)
    V(barrera)
    P(esperar[id])
}
```