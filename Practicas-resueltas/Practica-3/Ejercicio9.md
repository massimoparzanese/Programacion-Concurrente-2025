## Consigna

En un examen de la secundaria hay un preceptor y una profesora que deben tomar un examen
escrito a 45 alumnos. El preceptor se encarga de darle el enunciado del examen a los alumnos
cundo los 45 han llegado (es el mismo enunciado para todos). La profesora se encarga de ir
corrigiendo los exámenes de acuerdo con el orden en que los alumnos van entregando. Cada
alumno al llegar espera a que le den el enunciado, resuelve el examen, y al terminar lo deja
para que la profesora lo corrija y le envíe la nota. Nota: maximizar la concurrencia; todos los
procesos deben terminar su ejecución; suponga que la profesora tiene una función
corregirExamen que recibe un examen y devuelve un entero con la nota.

## Resolucioon

```
Monitor Aula {
    int esperando = 0;
    text examenes[44];
    procedure llegada(id: in int, exam: out text){
        esperando ++;
        if(esperando == 45) signal(precep)
        wait(espera)
        exam = examenes[id];
    }

    procedure esperarAlu(exam: in text){
        if(esperando == 45){
            for i 0 to 44 {
                examen[i] = exam;
                signal(espera)
            }
        }
    }
}
```