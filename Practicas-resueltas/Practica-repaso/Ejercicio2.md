## Consigna

Implemente una solución para el siguiente problema. Un sistema debe validar un conjunto de 10000
transacciones que se encuentran disponibles en una estructura de datos. Para ello, el sistema dispone
de 7 workers, los cuales trabajan colaborativamente validando de a 1 transacción por vez cada uno.
Cada validación puede tomar un tiempo diferente y para realizarla los workers disponen de la
función Validar(t), la cual retorna como resultado un número entero entre 0 al 9. Al finalizar el
procesamiento, el último worker en terminar debe informar la cantidad de transacciones por cada
resultado de la función de validación. Nota: maximizar la concurrencia.

## Resolución

```
int N = 10000
sem posiciones[N] = ([N] 1);
int transacciones[N];
sem valores[10] = ([10] 1);
int resultados[10] = ([10] 0)
sem mutexIndice = 1;
sem mutex = 1
int total = 0;
process worker[id:0...6]{
    int valor;
    while(indice_local < N){
        P(mutex_indice);
        if (i < N) {
            indice_local = i;
            i = i + 1;
            V(mutex_indice);
            valor = Validar(transacciones[indice_local])
            P(valores[valor])
            resultados[valor] ++;
            V(valores[valor])

        } else {
            V(mutex_indice); 
        }
    }
    P(mutex)
    total +=1;
    if(total == 10) for i 1 to 10 Imprimir(resultados[i])
    V(mutex)
}
```