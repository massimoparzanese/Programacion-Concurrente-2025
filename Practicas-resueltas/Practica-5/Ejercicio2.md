## Consigna

Se quiere modelar el funcionamiento de un banco, al cual llegan clientes que deben realizar
un pago y retirar un comprobante. Existe un único empleado en el banco, el cual atiende de
acuerdo con el orden de llegada.
a) Implemente una solución donde los clientes llegan y se retiran sólo después de haber sido
atendidos.
b) Implemente una solución donde los clientes se retiran si esperan más de 10 minutos para
realizar el pago.
c) Implemente una solución donde los clientes se retiran si no son atendidos
inmediatamente.
d) Implemente una solución donde los clientes esperan a lo sumo 10 minutos para ser
atendidos. Si pasado ese lapso no fueron atendidos, entonces solicitan atención una vez más
y se retiran si no son atendidos inmediatamente.


## Resolución

### Inciso A


```ada
Procedure Banco is 
    
    Task type cliente;

    Task empleado is
        Entry pedidos(pedido: IN text, respuesta: OUT text);
    end empleado;

    arrCliente: array(1..N) of cliente;

    Task body empleado is
        ped: text;
    begin
        loop
            ACCEPT pedidos(ped,respuesta) is
                respuesta = resolverPedido(ped);
            end pedidos;
        end loop;
    end empleado;


    Task body cliente is
        pedido: text;
        r: text;
    begin
        pedido = ...; // genera pedido;
        empleado.pedidos(pedido,r);
    end cliente;
begin
    null
end banco;
```

### Inciso b

```ada
Procedure Banco is 
    
    Task type cliente;

    Task empleado is
        Entry pedidos(pedido: IN text, respuesta: OUT text);
    end empleado;

    arrCliente: array(1..N) of cliente;

    Task body empleado is
        ped: text;
    begin
        loop
            ACCEPT pedidos(ped: in text,respuesta: out text) do
                respuesta = resolverPedido(ped);
            end pedidos;
        end loop;
    end empleado;


    Task body cliente is
        pedido: text;
        r: text;
    begin
        pedido = ...; // genera pedido;
        SELECT 
            empleado.pedidos(pedido,r);
        OR DELAY 10 minutos
            null; // no se que poner, pero se va
        end SELECT;
    end cliente;
begin
    null
end banco;
```

### Inciso C


```ada
Procedure Banco is 
    
    Task type cliente;

    Task empleado is
        Entry pedidos(pedido: IN text, respuesta: OUT text);
    end empleado;

    arrCliente: array(1..N) of cliente;

    Task body empleado is
        ped: text;
    begin
        loop
            ACCEPT pedidos(ped: in text,respuesta: out text) do
                respuesta = resolverPedido(ped);
            end pedidos;
        end loop;
    end empleado;


    Task body cliente is
        pedido: text;
        r: text;
    begin
        pedido = ...; // genera pedido;
        SELECT 
            empleado.pedidos(pedido,r);
        ELSE 
            null; // no se que poner, pero se va
        end SELECT;
    end cliente;
begin
    null
end banco;
```

### Inciso D


```ada
Procedure Banco is 
    
    Task type cliente;

    Task empleado is
        Entry pedidos(pedido: IN text, respuesta: OUT text);
    end empleado;

    arrCliente: array(1..N) of cliente;

    Task body empleado is
        ped: text;
    begin
        loop
            ACCEPT pedidos(ped: in text,respuesta: out text) do
                respuesta = resolverPedido(ped);
            end pedidos;
        end loop;
    end empleado;


    Task body cliente is
        pedido: text;
        r: text;
    begin
        pedido = ...; // genera pedido;
        SELECT 
            empleado.pedidos(pedido,r);
        OR DELAY 10 minutos
            SELECT
                empleado.pedidos(pedido, r);
            ELSE
                null; -- se retira si no es atendido inmediatamente
            END SELECT;
        end SELECT;
    end cliente;
begin
    null
end banco;
```