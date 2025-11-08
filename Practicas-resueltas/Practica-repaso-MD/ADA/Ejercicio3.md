## Consigna
Resolver el siguiente problema. La oficina central de una empresa de venta de indumentaria debe
calcular cuántas veces fue vendido cada uno de los artículos de su catálogo. La empresa se compone
de 100 sucursales y cada una de ellas maneja su propia base de datos de ventas. La oficina central
cuenta con una herramienta que funciona de la siguiente manera: ante la consulta realizada para un
artículo determinado, la herramienta envía el identificador del artículo a las sucursales, para que cada
una calcule cuántas veces fue vendido en ella. Al final del procesamiento, la herramienta debe
conocer cuántas veces fue vendido en total, considerando todas las sucursales. Cuando ha terminado
de procesar un artículo comienza con el siguiente (suponga que la herramienta tiene una función
generarArtículo() que retorna el siguiente ID a consultar). Nota: maximizar la concurrencia. Existe
una función ObtenerVentas(ID) que retorna la cantidad de veces que fue vendido el artículo con
identificador ID en la base de la sucursal que la llama.


## Resolución

```ada
Procedure central

    Task type sucursal;

    Task herramienta is
        Entry pedirArticulo(art: out int);
        Entry devolverCantidad(art: in int, idP: in int);
    end herramienta;


    arraySucursal: array(1..100) of sucursal;

    Task body sucursal is
        articulo: int;
        cant: int;
    begin
        loop 
            herramienta.pedirArticulo(articulo);
            cant = ObtenerVentas(articulo);
            herramienta.devolverCantidad(cant, articulo);
            herramienta.seguir()
        end loop;
    end sucursal;

    Task body herramienta is
        sig: int;
        bufferCant: Cola;
        cantLocal: int;
    begin
        sig = generarArticulo();
        cantLocal:=0;
        while (sig <> -1) loop
            for i: 1 to 200 loop
                SELECT 
                    Accept pedirArticulo(art: out int) is
                        art = sig;
                    end pedirArticulo;
                OR
                    Accept devolverCantidad(art,idP) is
                        cantLocal:= cantLocal + art;
                    end devolverCantidad;
                end select; 
            end loop;
            push(bufferCant,(sig,cantLocal))
            sig = generarArticulo();
            cantLocal := 0;
            for i: 1 to 100 loop
                Accept seguir()
            end loop;
        end loop;
    end;

begin
end central;
```