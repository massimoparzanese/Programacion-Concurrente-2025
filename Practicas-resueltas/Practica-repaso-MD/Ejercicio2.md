## Consigna

Resolver el siguiente problema con PMS. En la estación de trenes hay una terminal de SUBE que
debe ser usada por P personas de acuerdo con el orden de llegada. Cuando la persona accede a la
terminal, la usa y luego se retira para dejar al siguiente. Nota: cada Persona usa sólo una vez la
terminal.

## Resolucion

```
process AdminSube {
    int idP;
    Cola pedidos;
    bool libre = true;
    do persona[*]?pedido(idP) -> push(pedidos, idP)
    □  !empty(pedidos) && libre; persona[*]?pedido(idP) ->
        libre = false; 
        idP = pop(pedidos);
        persona[idP]!pasar
    □ !empty(pedidos) ; persona[*]!salir() ->
        idP = pop(pedidos);
        persona[idP]!pasar
    □ empty(pedidos) ; persona[*]!salir() ->
        libre= true;
    od
}
process persona[id:0..P-1]{
    AdminSube!pedido(idP)
    AdminSube?pasar
    // usa terminal de sube
    AdminSube!salir
}
```