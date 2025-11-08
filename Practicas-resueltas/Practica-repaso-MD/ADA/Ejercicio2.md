## Consigna

Resolver el siguiente problema. En un negocio de cobros digitales hay P personas que deben pasar
por la única caja de cobros para realizar el pago de sus boletas. Las personas son atendidas de acuerdo
con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que
pagan más. Adicionalmente, las personas ancianas tienen prioridad sobre los dos casos anteriores.
Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los
recibos de pago.


## Resolución

```ada
Procedure Negocio

    Task type persona;

    Task type caja is
        Entry mayorPedidos(CantBoletas: in Integer;dinero: in real; Vuelto: out real; Recibos: out text)
        Entry menorPedidos(CantBoletas: in Integer;dinero: in real; Vuelto: out real; Recibos: out text)
        Entry ancianos(CantBoletas: in Integer;dinero: in real; Vuelto: out real; Recibos: out text)
    end;

    
    arrayPersonas: array(1..P) of persona;


    Task body persona 
        esAnciano: bool;
        cantBoleta: integer;
        dinero: real;
        vuelto: real;
        recibos: text
    begin
        cantBoletas := ...;
        esAnciano:= ...;
        dinero:= ...;   // variables ya inicializadas donde la persona sabe que es y cuanto trae
        if(esAnciano) Caja.ancianos(cantBoleta, dinero, vuelto, recibos);
        else if (cantBoleta < 5) Caja.menorPedidos(cantBoleta, dinero, vuelto, recibos);
        else Caja.mayorPedidos(cantBoleta, dinero, vuelto, recibos);
    end persona;

    Task body caja is
    begin
        loop
            SELECT 
                ACCEPT ancianos(CantBoletas: in Integer;dinero: in real; Vuelto: out real; Recibos: out text) is
                    Vuelto:= devolverVuelto(CantBoletas, dinero);
                    Recibos:= generarRecibo(CantBoletas,dinero, Vuelto);
                end ancianos
            OR 
                when(ancianos'count = 0) => Accept menorPedidos(CantBoletas: in Integer;dinero: in real; Vuelto: out real; Recibos: out text) is 
                    Vuelto:= devolverVuelto(CantBoletas, dinero);
                    Recibos:= generarRecibo(CantBoletas,dinero, Vuelto);
                end menorPedidos
            OR
                when(ancianos'count = 0 and menorPedidos'count = 0) => Accept mayorPedidos(CantBoletas: in Integer;dinero: in real; Vuelto: out real; Recibos: out text) is 
                    Vuelto:= devolverVuelto(CantBoletas, dinero);
                    Recibos:= generarRecibo(CantBoletas,dinero, Vuelto);
                end mayorPedidos    
            End select;
        end loop;
    end caja;
        
begin
end negocio
```