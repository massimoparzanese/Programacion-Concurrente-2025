## Consigna

Se dispone de un puente por el cual puede pasar un solo auto a la vez. Un auto pide permiso
para pasar por el puente, cruza por el mismo y luego sigue su camino.

```
Monitor Puente{
    cond cola;
    int cant= 0;
    Procedure entrarPuente (){
        while ( cant > 0) wait (cola);
        cant = cant + 1;
        end;
    }
    Procedure salirPuente (){
        cant = cant – 1;
        signal(cola);
        end;
    }
}
 Process Auto [a:1..M]{
    Puente. entrarPuente (a);
    “el auto cruza el puente”
    Puente. salirPuente(a);
 }
```

a. ¿El código funciona correctamente? Justifique su respuesta.
b. ¿Se podría simplificar el programa? ¿Sin monitor? ¿Menos procedimientos? ¿Sin variable condition? En caso afirmativo, rescriba el código.
c. ¿La solución original respeta el orden de llegada de los vehículos? Si rescribió el código en el punto b), ¿esa solución respeta el orden de llegada?


## Resolución

### A

Sí, funciona correctamente.

### B

Sí, se podría simplificar, sin una condition y con menos procedimientos.

```
Monitor Puente {
    Procedure UsarPuente () {
        "cruzar el puente"
    }
}

Process Auto [a:1..M] {
    Puente.UsarPuente();
    // después de cruzar, el proceso sigue su camino
}
```

Como no pide respetar el orden de llegada, se puede utilizar de esa forma

### C

La primera no respeta el orden de llegada porque tiene ciertos "errores", los cuales menciono en el inciso A. 