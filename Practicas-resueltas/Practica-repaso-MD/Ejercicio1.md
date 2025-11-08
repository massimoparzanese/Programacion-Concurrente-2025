## Consigna

En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras
compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera
impresora que se encuentre libre:
a) Implemente un programa que permita resolver el problema anterior usando PMA.
b) Resuelva el mismo problema anterior pero ahora usando PMS.

## Resolución

### PMA

```
chan pedidos(int,text);
chan respuestas[100](text);
Process Impresora[id:0..4]{
    int idP;
    text ped;
    text resultado;
    while (true){
        recieve pedidos(idP,ped);
        resultado = Imprimir(ped);
        send respuestas[idP](resultado)
    }
}
Process empleado[id:0..99]{
    text doc;
    text resp;
    doc = ...; // prepara su pedido de
    send pedidos(id,doc)
    recieve respuestas[id](resp)
}
```

### PMS

```
Process Admin {
    Cola buffer;
    int idP;
    int idImp;
    text pedido;
    do Empleado[*]?pedido(idP,pedido) → push(buffer, (idP,pedido));
    □ not empty(buffer); Impresora[*]?lista(idImp) →
        idP, pedido = pop(buffer)
        Impresora[idImp]!pedidoImp(idP,pedido)
    od

}
Process Impresora[id:0..4]{
    int idP;
    text ped;
    text resultado;
    while (true){
        Admin!lista(id);
        Admin?pedidoImp(idP, ped)
        resultado = Imprimir(ped);
        empleado[idP]!respuesta(resultado)
    }
}
Process empleado[id:0..99]{
    text doc;
    text resp;
    doc = ...; // prepara su pedido de
    Admin!pedidos(id,doc)
    recieve Impresora[*]?respuesta(resp)
}

```
