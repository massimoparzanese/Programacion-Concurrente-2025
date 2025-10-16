## Consigna

En un examen final hay N alumnos y P profesores. Cada alumno resuelve su examen, lo
entrega y espera a que alguno de los profesores lo corrija y le indique la nota. Los
profesores corrigen los exámenes respetando el orden en que los alumnos van entregando.
a) Considerando que P=1.
b) Considerando que P>1.
c) Ídem b) pero considerando que los alumnos no comienzan a realizar su examen hasta
que todos hayan llegado al aula.
Nota: maximizar la concurrencia; no generar demora innecesaria; todos los procesos deben terminar su ejecución

## Resolución


### Inciso A

```
process Admin {
    cola fila;
    text exam;
    int idA;
    for i = 1 to  N*2{
    if Alumno[*]?entregas(exam,idA) ->  push(fila,(exam,idA)); 
    □ not empty(fila); profesor?listo() -> 
        idA,exam = pop(fila)
        profesor!corregir(exam,idA);
    fi
    }

}
process profesor{
    int nota;
    int idA;
    text exam;
    for i 1 to N{
        Admin!listo()
        Admin?corregir(exam,idA)
        nota = corregir(exam);
        alumno[idA]!correccion(nota)
    }

}
process alumno[id: 0..N-1]{
    int nota;
    text exam;
    exam = resolver();
    Admin!entregas(exam,id)
    profesor?corregido(nota)
}
```

### Inciso B

```
process Admin {
    cola fila;
    text exam;
    int idA;
    int idP;
    int cantAvisos = 0;
    do Alumno[*]?entregas(exam,idA) ->  push(fila,(exam,idA)); 
     □ not empty(fila); profesor[*]?listo(idP) -> 
        idA,exam = pop(fila)
        profesor[idP]!corregir(exam,idA);
        examenes_procesados++;
    □ empty(fila);  examenes_procesados == N && cantAvisos < P; 
        Profesor[*]?listo(idP) -> 
        profesor[idP]!corregir("vacio",-1);
        cantAvisos++;
    od
    

}
process profesor [0..P-1]{
    int nota;
    int idA;
    text exam;
    bool terminamos
    while(not terminamos){
        Admin!listo(id); // Pide el siguiente examen
        if Admin?corregir(exam, idA) ->
            if(exam == vacio) terminamos = true;
            else{
                nota = corregir(exam); 
                Alumno[idA]!correccion(nota);
            }
    }

}
process alumno[id: 0..N-1]{
    int nota;
    text exam;
    exam = resolver();
    Admin!entregas(exam,id)
    profesor[*]?corregido(nota)
}
```

### Inciso C

```
process Barrera {
    int cant = 0;
    do Alumno[*]?llegada
}
process Admin {
    cola fila;
    text exam;
    int idA;
    int idP;
    do Alumno[*]?entregas(exam,idA) ->  push(fila,(exam,idA)); 
     □ not empty(fila); profesor[*]?listo(idP) -> 
        idA,exam = pop(fila)
        profesor[idP]!corregir(exam,idA);
        examenes_procesados++;
    □ empty(fila);  examenes_procesados == N; 
        Profesor[*]?listo(idP) -> 
        profesor[idP]!corregir("vacio",-1);
    od

}
process profesor [0..P-1]{
    int nota;
    int idA;
    text exam;
    bool terminamos
    while(not terminamos){
        Admin!listo(id); // Pide el siguiente examen
        if Admin?corregir(exam, idA) ->
            if(exam == vacio) terminamos = true;
            else{
                nota = corregir(exam); 
                Alumno[idA]!correccion(nota);
            }
    }

}
process alumno[id: 0..N-1]{
    int nota;
    text exam;
    exam = resolver();
    Admin!entregas(exam,id)
    profesor[*]?corregido(nota)
}
```
