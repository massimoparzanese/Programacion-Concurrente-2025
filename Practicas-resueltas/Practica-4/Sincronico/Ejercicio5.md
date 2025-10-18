## Consigna

En un estadio de fútbol hay una máquina expendedora de gaseosas que debe ser usada por
E Espectadores de acuerdo con el orden de llegada. Cuando el espectador accede a la
máquina en su turno usa la máquina y luego se retira para dejar al siguiente. Nota: cada
Espectador una sólo una vez la máquina.

## Resolución

```
process admin {
    int idP;
    cola buffer;
    bool libre = true;
    do espectador[*]?pedido(idP) -> push(buffer, idP)
    □ empty(buffer); espectador[*]?salir->
        libre = true;
    □ libre && not empty(buffer); espectador[*]?pedido(idP) → 
        libre = false; 
        pop(buffer, idP);
        Espectador[idP]!pasar();
    □ not empty(buffer); espectador[*]?salir() ->
        idP = pop(buffer)
        espectador[idP]!pasar()
}
process espectador [id: 0..E-1]{
    admin!pedido(id)
    admin?pasar()
    admin!salir()
}
```