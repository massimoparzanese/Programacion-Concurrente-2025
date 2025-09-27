## Consigna
En un parque hay un juego para ser usada por N personas de a una a la vez y de acuerdo al
orden en que llegan para solicitar su uso. Además, hay un empleado encargado de desinfectar el
juego durante 10 minutos antes de que una persona lo use. Cada persona al llegar espera hasta
que el empleado le avisa que puede usar el juego, lo usa por un tiempo y luego lo devuelve.
Nota: suponga que la persona tiene una función Usar_juego que simula el uso del juego; y el
empleado una función Desinfectar_Juego que simula su trabajo. Todos los procesos deben
terminar su ejecución.

>[!Important]
>Consultar.
## Resolución

```
Monitor juego {
    cond usar;
    cond avisoEncargado;
    cond esperar;
    int esperando;
    bool libre = true;
    procedure pedir(){
        if(libre){
            signal(avisoEncargado)
            libre = false;
        }
        esperando ++;
        wait(esperar)
    }

    procedure esperarAviso(){
        if(esperando == 0) {
            libre = true;
            wait(avisoEncargado)
        }
    }

    procedure concederPaso(){
        signal(esperar);
        esperando --;
        wait(usar)
    }

    procedure termine(){
        signal(usar);
    }
}
process persona[id:0..N-1]{
    juego.pedir()
    Usar_juego()
    juego.termine()
}
process encargado{
    for i 1 to N{
        juego.esperarAviso()
        Desinfectar_juego();
        juego.concederPaso()
    }
}
```