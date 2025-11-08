## Consigna

Resolver el siguiente problema. La página web del Banco Central exhibe las diferentes cotizaciones
del dólar oficial de 20 bancos del país, tanto para la compra como para la venta. Existe una tarea
programada que se ocupa de actualizar la página en forma periódica y para ello consulta la cotización
de cada uno de los 20 bancos. Cada banco dispone de una API, cuya única función es procesar las
solicitudes de aplicaciones externas. La tarea programada consulta de a una API por vez, esperando
a lo sumo 5 segundos por su respuesta. Si pasado ese tiempo no respondió, entonces se mostrará
vacía la información de ese banco.

## Resolución

```ada
Process Banco {

    Task procesador;
        

    Task type Api is
        Entry solicitudes(resp: out text);
    end api;

    ArrAPIs: array (1..20) of Api;
    Task body procesador is
        bancosResp: array(1..20) of respuesta;
        resp: text;
    begin
        for i 1 to 20 loop
            SELECT ArrAPIs(i).solicitudes(resp);
            OR DELAY 5 segundos
                resp = '';
            end select;
            bancosResp(i) = resp;
        end loop;
    end procesador

    Task body Api is 
    begin
        loop
            Accept solicitudes(resp: out text) do
                resp = devolverCotizacion(); // devuelve la cotizacion especifica de ese banco
            end solicitudes;
        end loop;
    end Api;
    begin 
    end banco;
}
```