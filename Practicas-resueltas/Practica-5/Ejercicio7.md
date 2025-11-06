## Consigna

Hay un sistema de reconocimiento de huellas dactilares de la policía que tiene 8 Servidores
para realizar el reconocimiento, cada uno de ellos trabajando con una Base de Datos propia;
a su vez hay un Especialista que utiliza indefinidamente. El sistema funciona de la siguiente
manera: el Especialista toma una imagen de una huella (TEST) y se la envía a los servidores
para que cada uno de ellos le devuelva el código y el valor de similitud de la huella que más
se asemeja a TEST en su BD; al final del procesamiento, el especialista debe conocer el
código de la huella con mayor valor de similitud entre las devueltas por los 8 servidores.
Cuando ha terminado de procesar una huella comienza nuevamente todo el ciclo. Nota:
suponga que existe una función Buscar(test, código, valor) que utiliza cada Servidor donde
recibe como parámetro de entrada la huella test, y devuelve como parámetros de salida el
código y el valor de similitud de la huella más parecida a test en la BD correspondiente.
Maximizar la concurrencia y no generar demora innecesaria.

## Resolución

```ada
Procedure sistema is

    Task type servidor;

    Task especialista is
        Entry imagen(img: out text);
        Entry resultados(cod: in int, valor: in int);
        Entry seguir()
    end especialista;


    arrayServidores: array(1.8) of servidor;

    Task body especialista is
        valor: int;
        codigo: int;
        test: text;
        auxCod: int;
        auxValor: int;
    begin
        valor:=0;
        codigo:=0;
        loop
            test: ..; // toma imagen de huella
            for i: 1 to 16
                SELECT 
                    Accept imagen(img: out text) do
                        img:= test;
                    end imagen;
                OR
                    Accept resultados(auxCod, auxValor);
                    if(auxValor >= valor) then
                        valor = auxValor;
                        cod = auxCod;
                    end if;
                end select;
            end loop;
            for i 1 to 8 loop;
                Accept seguir();
            end loop;
        end loop;
    end especialista;

    Task body servidor is
        cod: int
        img: text;
        valor: int;
    begin
        loop
            Especialista.imagen(img);
            Buscar(img,cod,valor)
            Especialista.resultados(cod,valor);
            Especialista.seguir()
        end loop;
    end servidor;
begin
end sistema;
```