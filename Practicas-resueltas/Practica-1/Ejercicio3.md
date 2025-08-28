## Consigna

3. Dada la siguiente solución de grano grueso:
    a) Indicar si el siguiente código funciona para resolver el problema de
    Productor/Consumidor con un buffer de tamaño N. En caso de no funcionar, debe
    hacer las modificaciones necesarias.
```
int cant = 0;
int pri_ocupada = 0;
int pri_vacia = 0;
int buffer[N];
Process Productor::
{
    while (true) 
    { *produce elemento*
        <await (cant < N); cant++>
        buffer[pri_vacia] = elemento;
        pri_vacia = (pri_vacia + 1) mod N;
    }
}
Process Consumidor 
{
    while (true)
    {
        <await (cant > 0); cant-->
        elemento = buffer[pri_ocupada];
        pri_ocupada = (pri_ocupada + 1 ) mod N
        *consume elemento*
    }
}
```
    b) Modificar el código para que funcione para C consumidores y P productores.

## Respuestas

**a)** No es correcto ya que ambos "modifican" o "consumen" del buffer a la vez, y puede que cuando el consumidor quiera consumir del buffer, todavía el productor no colocó el elemento en el
Solución:
```
int cant = 0;
int pri_ocupada = 0;
int pri_vacia = 0;
int buffer[N];
Process Productor::
{
    while (true) 
    { *produce elemento*
        <await (cant < N); 
        buffer[pri_vacia] = elemento;
        cant++>
        pri_vacia = (pri_vacia + 1) mod N;
    }
}
Process Consumidor::
{
    while (true)
    {
        <await (cant > 0); 
        elemento = buffer[pri_ocupada];
        cant-->
        pri_ocupada = (pri_ocupada + 1 ) mod N
        *consume elemento*
    }
}
```
**b)**
```
int cant = 0;
int pri_vacia = 0;
int pri_ocupada = 0;
int buffer[N];

Process Productor[id: 0..N-1] {
    while (true) {
        // produce elemento
        < await (cant < N);
          buffer[pri_vacia] = elemento;
          pri_vacia = (pri_vacia + 1) mod N;
          cant++;
        >
    }
}

Process Consumidor[id: 0..N-1] {
    while (true) {
        < await (cant > 0);
          elemento = buffer[pri_ocupada];
          pri_ocupada = (pri_ocupada + 1) mod N;
          cant--;
        >
        // consume elemento
    }
}

```
