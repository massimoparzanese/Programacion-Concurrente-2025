## Consigna

Resolver el siguiente problema. En una empresa trabajan 20 vendedores ambulantes que forman 5
equipos de 4 personas cada uno (cada vendedor conoce previamente a qué equipo pertenece). Cada
equipo se encarga de vender un producto diferente. Las personas de un equipo se deben juntar antes
de comenzar a trabajar. Luego cada integrante del equipo trabaja independientemente del resto
vendiendo ejemplares del producto correspondiente. Al terminar cada integrante del grupo debe
conocer la cantidad de ejemplares vendidos por el grupo. Nota: maximizar la concurrencia.

## Resolución

```
Monitor Equipo[id: 0..4]{
    cond companieros;
    int cant = 0;

    procedure juntarse(){
        cant++;
        if(cant == 4) signal_all(companieros)
        else wait(companieros)
    }
     procedure actualVendidos(vendidos: in int, total: out int){
        cant += vendidos;
        if(cantCompañeros == 4){
            signal_all(companieros)
        }
        else{
            wait(companieros)
        }
        total = cant;
    }
}
procedure vendedores[id:0..19]{
    int num =..; // numero de equipo que sabe
    Equipo[num].juntarse()
    int ventas = 0;
    while(cond) // no se que poner porque no se hasta cuando vende
    {
        // realiza venta
        venta += 1;
    }
    int cant;
    Ventas.actualVendidos(ventas,cant)
}
```