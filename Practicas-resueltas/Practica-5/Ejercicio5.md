## Consigna

En una playa hay 5 equipos de 4 personas cada uno (en total son 20 personas donde cada
una conoce previamente a que equipo pertenece). Cuando las personas van llegando
esperan con los de su equipo hasta que el mismo esté completo (hayan llegado los 4
integrantes), a partir de ese momento el equipo comienza a jugar. El juego consiste en que
cada integrante del grupo junta 15 monedas de a una en una playa (las monedas pueden ser
de 1, 2 o 5 pesos) y se suman los montos de las 60 monedas conseguidas en el grupo. Al
finalizar cada persona debe conocer el grupo que más dinero junto. Nota: maximizar la
concurrencia. Suponga que para simular la búsqueda de una moneda por parte de una
persona existe una función Moneda() que retorna el valor de la moneda encontrada.

## Resolución

```ada
procedure playa is

    Task type barrera is 
        Entry llegada(id: in int);
        Entry resultados(resul: in int);
        Entry espera(max: in int);
    end barrera;

    Task type persona is 
        Entry esperarCompañeros;
        Entry identificacion(i: in int);
        Entry resultadoFinal(max: in int);
    end persona;

    Task coordinadorGeneral is
        Entry puntajes(puntaje: in int);
    end coordinadorGeneral;


    arrayPersonas: array(1.20) of persona;
    arrayBarrera: array(1..5) of barrera;


    Task body coordinadorGeneral is
        auxMax: int;
    begin
        auxMax = 0;
        for i: 1 to 5 loop
            Accept puntajes(max: in int);
            if(max > auxMax) auxMax = max;
        end loop;
        for i: 1 to 5 loop
            arrayBarrera(i).espera(auxMax);
        end loop;
    end coordinadorGeneral;

    Task body barrera is
        aux: int;
        buffer: cola; 
        auxId: int;
    begin
        aux:= 0;
        for i: 1 to 5 loop // espero a que lleguen todos
            Accept llegada(id: in int)do
                push(buffer,id) // me guardo los id de los de mi equipo
            end llegada;
        end loop;
        for i: 1 to 5 loop // Les aviso que pueden seguir
            auxId = pop(buffer);
            arrayPersonas(auxId).esperarCompañeros;
        end loop;
    end barrera;


begin 
    for i 1 to 20 loop
        arrayPersonas(i).identificacion(i)
    end loop;
end playa;
```