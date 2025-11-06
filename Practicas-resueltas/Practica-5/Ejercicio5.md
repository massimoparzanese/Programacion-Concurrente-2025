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
        Entry llegada();
        Entry esperarCompañeros;
        Entry resultados(resul: in int);
        Entry resultadoFinal(max: out int);
    end barrera;

    Task type persona;

    Task coordinadorGeneral is 



    arrayPersonas: array(1.20) of persona;
    arrayBarrera: array(1..5) of barrera;


    Task body coordinadorGeneral is
        auxMax: int;
    begin
        auxMax = 0;
        for i: 1 to 5 loop
            Accept maximo(max: in int)do
                if(max > auxMax) auxMax = max;
        end loop;
        for i: 1 to 5 loop
            Accept espera(max: out int)do
                max = auxMax;
            end espera;
        end loop;
    end coordinadorGeneral;

    Task body barrera is
        aux: int;
        auxMax: int;
    begin
        aux:= 0;
        for i: 1 to 5 loop // espero a que lleguen todos
            Accept llegada();
        end loop;
        for i: 1 to 5 loop // Les aviso que pueden seguir
            Accept esperarCompañeros();
        end loop;
        for i: 1 to 5 loop // recibo resultados de juntar monedas
            Accept resultados(resul: in int) do
                aux:= aux + resul;
            end resultados;
        end loop;
        
        coordinadorGeneral.maximo(aux);
        coordinadorGeneral.espera(auxMax);

        for i: 1 to 5 loop // Les aviso que pueden seguir
            Accept resultadoFinal(max: out int) do
                max = auxMax;
            end resultadoFinal;
        end loop;
    end barrera;

    Task body persona is
        recaudado: int;
        equipo: int;
        aux: int
    begin
        recaudado:= 0;
        equipo: ...; // se le asigna su equipo que ya ocnoce
        arrayBarrera(equipo).llegada();
        arrayBarrera(equipo).esperarCompañeros();
        for i: 1 to 15 loop
            aux = Moneda();
            recaudado:= recaudado + aux;
        end loop;
        arrayBarrera(equipo).resultados(recaudado);
        arrayBarrera(equipo).resultadoFinal(recaudado);
    end persona;

begin 

end playa;
```
// Hacer que la barrera acepte siempre y el coordinador general le envie el max a las barreras