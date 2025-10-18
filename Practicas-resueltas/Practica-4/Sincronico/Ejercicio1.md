## Consigna

Suponga que existe un antivirus distribuido que se compone de R procesos robots
Examinadores y 1 proceso Analizador. Los procesos Examinadores están buscando
continuamente posibles sitios web infectados; cada vez que encuentran uno avisan la
dirección y luego continúan buscando. El proceso Analizador se encarga de hacer todas las
pruebas necesarias con cada uno de los sitios encontrados por los robots para determinar si
están o no infectados.
a) Analice el problema y defina qué procesos, recursos y comunicaciones serán
necesarios/convenientes para resolverlo.
b) Implemente una solución con PMS sin tener en cuenta el orden de los pedidos.
c) Modifique el inciso (b) para que el Analizador resuelva los pedidos en el orden
en que se hicieron.

## Resolución

### Inciso A

Se requieren dos tipos de procesos para resolverlo, pero en caso de querer maximizar la concurrencia/mantener orden serán:

- Examinador[id: 0 ..R-1]: El arreglo de procesos robot que buscan continuamente y reportan sitios.

- Analizador: El único proceso encargado de probar y determinar si los sitios están infectados.

- Admin: Un proceso intermedio que funciona como una cola (buffer). Su función es desacoplar a los Examinadores del Analizador , permitiendo a los R Examinadores enviar sus reportes y continuar su trabajo inmediatamente.



### Inciso B

```
process admin {
    text url;
     cola Buffer; 
    do Examinador[*]?reportar(url) ->  push(Buffer, url); // Siempre acepta reportes para no bloquear a los Examinadores
    □ not empty(Buffer); Analizador?pedido() -> 
        url = pop(Buffer);
        Analizador!url(url); 
     od
}
process examinador[id:0..N-1]{
    text url;
    while (true){
        url = ...; // url sitio a analizar
        // examina sitio
        admin!examinar(url);
    }
}
process Analizador {
    text urlAux;
    while (true) {
        admin!pedido()
        admin?url(urlAux)
        resultado = analizar_sitio(urlAux); 
    }
}
```
### Inciso c

```
process admin {
    text url;
     cola Buffer; 
    do Examinador[*]?reportar(url) ->  push(Buffer, url); // Siempre acepta reportes para no bloquear a los Examinadores
    □ not empty(Buffer); Analizador?pedido() -> 
        url = pop(Buffer);
        Analizador!url(url); 
     od
}
process examinador[id:0..N-1]{
    text url;
    while (true){
        url = ...; // url sitio a analizar
        // examina sitio
        admin!examinar(url);
    }
}
process Analizador {
    text urlAux;
    while (true) {
        admin!pedido()
        admin?url(urlAux)
        resultado = analizar_sitio(urlAux); 
    }
}
```