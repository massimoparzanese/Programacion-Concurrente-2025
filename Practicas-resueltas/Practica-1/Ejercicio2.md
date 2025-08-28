## Consigna
Realice una solución concurrente de grano grueso (utilizando <> y/o <await B; S>) para el siguiente problema. Dado un número N verifique cuántas veces aparece ese número en un arreglo de longitud M. Escriba las pre-condiciones que considere necesarias.
---
precondiciones:
- array inicializado con M elementos.

- num inicializado.

- Todos los procesos saben el valor de num y la referencia de array.

### Resolución 
Se divide en una cantidad determinada de bloques para no crear M procesos ya que si M es muy grande puede requerir muchos procesos
precondiciones: 
- M > N
- Array inicializado
- M es divisible por N
```
int total = 0;
int bloque = M / N; // tamaño de cada bloque 

Process verificar_aparicion[id:0..N-1]
{
    int parcial = 0;
    int inicio = id * bloque;
    int fin = inicio + bloque;

    for (int j = inicio; j < fin; j++) {
        if (array[j] == num)
            parcial++;
    }

    <total = total + parcial;>
}

```