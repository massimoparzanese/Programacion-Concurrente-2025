## Consigna

En un examen de la secundaria hay un preceptor y una profesora que deben tomar un examen
escrito a 45 alumnos. El preceptor se encarga de darle el enunciado del examen a los alumnos
cundo los 45 han llegado (es el mismo enunciado para todos). La profesora se encarga de ir
corrigiendo los exámenes de acuerdo con el orden en que los alumnos van entregando. Cada
alumno al llegar espera a que le den el enunciado, resuelve el examen, y al terminar lo deja
para que la profesora lo corrija y le envíe la nota. Nota: maximizar la concurrencia; todos los
procesos deben terminar su ejecución; suponga que la profesora tiene una función
corregirExamen que recibe un examen y devuelve un entero con la nota.


>![Important]
>Consultar ejercicio.
## Resolucioon

```
Monitor Aula {
    int esperando = 0;
    text examenes[44];
    cond espera
    cond esperarAlumnos
    procedure llegada(id: in int, exam: out text){
        esperando ++;
        if(esperando == 45) signal(precep)
        wait(espera)
        exam = examenes[id];
    }

    procedure esperarAlu(){
        if(esperando < 45) wait(esperarAlumnos)
    }

    procedure darExamen(idA: in int, exam: in text){
        examenes[idA] = exam;
        signal(espera)
    }

}
Monitor Escritorio {
    cond esperarNota
    int notas[44];
    cola examen;
    cond esperarAlumnos
    procedure darExamen(idA: in int, exam: in text, nota: out int){
        push(cola,(idA,exam))
        signal(esperarAlumnos)
        wait(esperaNota)
        nota = notas[idA];
    }

    procedure tomarExamen(exam: out text, idA: out int){
        if(empty(cola)) wait(esperarAlumnos)
        exam,idA = pop(cola);
    }

    procedure dejarNota(nota: in int, idA: in int){
        notas[idA] = nota;
        signal(esperaNota)
    }
}
process profesora {
    for i 1 to 45{
        int idA;
        text exam;
        int nota;
        Escritorio.tomarExamen(exam,idA)
        nota = corregirExamen(exam);
        Escritorio.dejarNota(nota, idA)
    }
}
process preceptor {
    text exam; // examen inicializado con algun valor especifico
    Aula.esperarAlu()
    for i 0 to 44 {
        Aula.darExamen(i,exam);
    }
}
process Alumno[id:0..44]{
    text exam;
    int nota = 1;
    Aula.llegada(id,exam)
    // resuelve examen
    Escritorio.darExamen(id,exam,nota)
}
```