## Consigna

Se debe calcular el valor promedio de un vector de 1 millón de números enteros que se
encuentra distribuido entre 10 procesos Worker (es decir, cada Worker tiene un vector de
100 mil números). Para ello, existe un Coordinador que determina el momento en que se
debe realizar el cálculo de este promedio y que, además, se queda con el resultado. Nota:
maximizar la concurrencia; este cálculo se hace una sola vez.


## Resolución


```ada
Procedure programa is


    Task coordinador is
        Entry inicio;
        Entry resultado(resul: in int);
    Task type worker;

    arrayWorkers: array(1..10) of worker;


    Task body coordinador is
        aux: int;
        valorFinal: int;
    begin
        // espera un tiempo
        valorFinal:=0;
        for i 1 to 10 loop
            Accept inicio;
        end loop;
        for i 1 to 10 loop
            Accept resultado(resul: in int) do
                aux = resul;
            end resul;
            valorFinal:= valorFinal + aux;
        end loop;
        valorFinal:= valorFinal / 10;
    end coordinador;

    Task body worker is
        valor: int;
        aux: int;
        v: array(1.100000) of int;
    begin
        coordinador.inicio;
        valor:= 0;
        for i: 1 to 100000 loop;
            aux:= v[i];
            valor:= valor + aux;
        end loop;
        coordinador.resultado(valor);
    end worker;
begin
end programa;
```