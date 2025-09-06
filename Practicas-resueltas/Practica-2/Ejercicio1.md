## Consigna

1. Existen N personas que deben ser chequeadas por un detector de metales antes de poder
ingresar al avión.
a. Analice el problema y defina qué procesos, recursos y semáforos/sincronizaciones
serán necesarios/convenientes para resolverlo.
b. Implemente una solución que modele el acceso de las personas a un detector (es decir,
si el detector está libre la persona lo puede utilizar; en caso contrario, debe esperar).
c. Modifique su solución para el caso que haya tres detectores.
d. Modifique la solución anterior para el caso en que cada persona pueda pasar más de
una vez, siendo aleatoria esa cantidad de veces.

## Resolución

### A
Debería tener N procesos persona, un semáforo que será para usar el detector de metales.

### B
```
sem detector = 1;
process persona[id:0..n-1]{
    // llega al detector
    P(detector) // espera en caso de estar ocupado
    // realiza el proceso
    V(detector)
}
```

### C
```
sem detectore = 3;
process persona[id:0..n-1]{
    // llega al detector
    P(detector) // espera en caso de estar ocupado
    // realiza el proceso
    V(detector)
}
```

### D
```
sem detectore = 3;
int MAX = 15;
process persona[id:0..n-1]{
    // llega al detector
    int veces = random(1, MAX);
    for i = 1 to veces{
        P(detector) // espera en caso de estar ocupado
        // realiza el proceso
        V(detector)
    }
}
```
```