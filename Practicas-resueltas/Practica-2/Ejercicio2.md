## Consigna

Un sistema de control cuenta con 4 procesos que realizan chequeos en forma
colaborativa. Para ello, reciben el historial de fallos del día anterior (por simplicidad, de
tamaño N). De cada fallo, se conoce su número de identificación (ID) y su nivel de
gravedad (0=bajo, 1=intermedio, 2=alto, 3=crítico). Resuelva considerando las siguientes
situaciones:
    a) Se debe imprimir en pantalla los ID de todos los errores críticos (no importa el
    orden).
    b) Se debe calcular la cantidad de fallos por nivel de gravedad, debiendo quedar los
    resultados en un vector global.
    c) Ídem b) pero cada proceso debe ocuparse de contar los fallos de un nivel de
    gravedad determinado.

## Resolución

a.
```
sem mutexPrint = 1
Fallo historial[N];
int bloque = N / 4
prcoess colaborador[id:0..3]{
    int inicio = id * bloque;
    int fin = inicio + bloque;
    for (int j = inicio; j < fin; j++) {
        if (array[j].id == 3){
            P(mutexPrint)
            // imprime
            V(mutexPrint)
        }
    }
}

```
b.
```
sem mutexNivel[4] = ([4] 1);    
int contador[4] = ([4] 0); 
Fallo historial[N];
int bloque = N / 4
prcoess colaborador[id:0..3]{
    int inicio = id * bloque;
    int fin = inicio + bloque;
    for (int j = inicio; j < fin; j++) {
        int nivel = historial[j].nivel;
        P(mutexNivel[nivel]);                  // Bloqueo solo el contador del nivel
        contador[nivel] = contador[nivel] + 1; // Incremento seguro
        V(mutexNivel[nivel]);
    }
}

```

c.
```
sem mutexNivel[N] = ([N] 1);    
int contador[4] = ([4] 0); 
Fallo historial[N];
prcoess colaborador[id:0..3]{
    int nivel; // se le asigna un nivel al proceso
    int nivelhis;
    for (int j = 0; j < n; j++) {
        P(mutexNivel[j]);
            nivelhis = historial[j].nivel;
            if(nivel == nivelhis){
                contador[nivel] = contador[nivel] + 1; // Incremento seguro
            }
        V(mutexNivel[j]);
    }
}

```