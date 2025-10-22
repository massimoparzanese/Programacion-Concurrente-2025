## Consigna

Se requiere modelar un puente de un único sentido que soporta hasta 5 unidades de peso. El peso de los vehículos depende del tipo: cada auto pesa 1 unidad, cada camioneta pesa 2 unidades y cada camión 3 unidades. Suponga que hay una cantidad innumerable de vehículos (A autos, B camionetas y C camiones). Analice el problema y defina qué tareas, recursos y sincronizaciones serán necesarios/convenientes para resolverlo.
a. Realice la solución suponiendo que todos los vehículos tienen la misma prioridad.
b. Modifique la solución para que tengan mayor prioridad los camiones que el resto de los
vehículos.

## Resolución


### Inciso A

```ada
Procedure Puente is

    Task type auto;
    Task type camioneta;
    Task type camion;

    Task administrador is
        Entry pedidoA;
        Entry pedidoB;
        Entry pedidoC;
        Entry salidas(peso: in int);
    end administrador;

    arrAutos: array(1..A) of auto;
    arrCamionetas: array(1..B) of camioneta;
    arrCamiones: array(1..C) of camiones;

    Task body auto is
    Begin
        Administrador.pedidoA;
        // pasa por el puente
        Administrador.salidas(1);
    end auto;

    Task body camioneta is
    Begin
        Administrador.pedidoB;
        // pasa por el puente
        Administrador.salidas(2);
    end camioneta;

    Task body camion is
    Begin
        Administrador.pedidoC;
        // pasa por el puente
        Administrador.salidas(3);
    end camion;

    Task Body administrador is
        pesoRestante: int;
        aux: int;
    Begin
        pesoRestante := 5;
        loop
            SELECT 
                when(pesoRestante - 1 >= 0) => ACCEPT pedidoA do
                                                  pesoRestante = pesoRestante - 1;
                                                END pedidoA;
            OR
                when(pesoRestante - 2 >= 0) => ACCEPT pedidoB do
                                                  pesoRestante = pesoRestante - 2;
                                                END pedidoB;    
            OR   
                when(pesoRestante - 3 >= 0) => ACCEPT pedidoC do
                                                  pesoRestante = pesoRestante - 3;
                                                END pedidoC;
            ELSE 
                ACCEPT salidas(aux) do
                    pesoRestante = pesoRestante + aux;
                END salidas;
            END SELECT;
        END LOOP;
    End administrador;
Begin
    null;
end Puente;
```

### Inciso B


```ada
Procedure Puente is

    Task type auto;
    Task type camioneta;
    Task type camion;

    Task administrador is
        Entry pedidoA;
        Entry pedidoB;
        Entry pedidoC;
        Entry salidas(peso: in int);
    end administrador;

    arrAutos: array(1..A) of auto;
    arrCamionetas: array(1..B) of camioneta;
    arrCamiones: array(1..C) of camiones;

    Task body auto is
    Begin
        Administrador.pedidoA;
        // pasa por el puente
        Administrador.salidas(1);
    end auto;

    Task body camioneta is
    Begin
        Administrador.pedidoB;
        // pasa por el puente
        Administrador.salidas(2);
    end camioneta;

    Task body camion is
    Begin
        Administrador.pedidoC;
        // pasa por el puente
        Administrador.salidas(3);
    end camion;

    Task Body administrador is
        pesoRestante: int;
        aux: int;
    Begin
        pesoRestante := 5;
        loop
            SELECT
                when(pesoRestante - 3 >= 0)=> ACCEPT pedidoC do
                                                pesoRestante = pesoRestante - 3;
                                              END pedidoC;
            OR
                when(pedidoC'count = 0 and pesoRestante - 1 >= 0) => ACCEPT pedidoA do
                                                                        pesoRestante = pesoRestante - 1;
                                                                       END pedidoA;
            OR
                when(pedidoC'count = 0 and pesoRestante - 2 >= 0) => ACCEPT pedidoB do
                                                                        pesoRestante = pesoRestante - 2;
                                                                       END pedidoB;    
            ELSE 
                ACCEPT salidas(aux) do
                    pesoRestante = pesoRestante + aux;
                END salidas;
            END SELECT;
        END LOOP;
    End administrador;
Begin
    null;
end Puente;
```
