## Consigna

Se debe modelar el funcionamiento de una casa de comida rápida, en la cual trabajan 2 cocineros y 3 vendedores, y que debe atender a C clientes. El modelado debe considerar
que:
- Cada cliente realiza un pedido y luego espera a que se lo entreguen.
- Los pedidos que hacen los clientes son tomados por cualquiera de los vendedores y se lo pasan a los cocineros para que realicen el plato. Cuando no hay pedidos para atender, los vendedores aprovechan para reponer un pack de bebidas de la heladera (tardan entre 1 y 3 minutos para hacer esto).
- Repetidamente cada cocinero toma un pedido pendiente dejado por los vendedores, lo cocina y se lo entrega directamente al cliente correspondiente.
Nota: maximizar la concurrencia.

## Resolucion

```
chan espera(int,text)
chan esperaEntrega[0..C-1](text)
chan pedidos[0..2](int,text);
chan listaCocineros(int,text)
chan listo(int);
process Coordinador{
    int idV;
    int idP;
    text pedido;
    while (true){
        recieve listo(idV);
        if(empty(espera)) {idP = -1, pedido = "vacio"}
        else {
            recieve espera(idP,pedido)
        }
        end pedidos[idV](idP,pedido)
    }
}
process Cocinero[id:0..1]{
    int idP;
    text pedido;
    text plato;
    while (true){
        recieve listaCocineros(idP,pedido)
        plato = generarPlato(pedido); // cocina el plato pedido
        send esperaEntrega[idP](plato);
    }
}
process vendedor[id:0..2]{
    int idP;
    text pedido;
    while(true){
        send listo(id)
        recieve pedidos[id](idP,pedido)
        if(pedido == 'vacio'){
            delay(1 a 3 mins) // repone bebidas heladera
        }
        else {
            send listaCocineros(idP,pedido);
        }
    }
}
process cliente[id:0..C-1]{
    text pedido = ...; // pedido de plato
    text plato;
    send espera(id,pedido);
    recieve esperaEntrega[id](plato);
}
```