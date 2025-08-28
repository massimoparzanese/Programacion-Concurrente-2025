## Consigna

Dada la siguiente solución para el Problema de la Sección Crítica entre dos procesos
(suponiendo que tanto SC como SNC son segmentos de código finitos, es decir que terminan
en algún momento), indicar si cumple con las 4 condiciones requeridas:

```
int turno = 1;
Process SC1::
{ while (true)
    { while (turno == 2) skip;
    SC;
    turno = 2;
    SNC;
    }
}
Process SC2::
{ while (true)
    { while (turno == 1) skip;
    SC;
    turno = 1;
    SNC;
    }
}
```

Las 4 propiedades son las siguientes:
- Exclusión mutua: A lo sumo un proceso está en su SC.
- Ausencia de Deadlock (Livelock): si 2 o más procesos tratan de entrar a sus SC, al menos uno
tendrá éxito.
- Ausencia de Demora Innecesaria: si un proceso trata de entrar a su SC y los otros están en sus
SNC o terminaron, el primero no está impedido de entrar a su SC.
- Eventual Entrada: un proceso que intenta entrar a su SC tiene posibilidades de hacerlo
(eventualmente lo hará).

## Respuestas

La propiedad de Ausencia de demora innecesaria se viola, porque P2 puede querer entrar, cuando aún el P1 no llegó ni uso la CPU, y al no cambiar el valor del turno el p2 estará innecesariamente demorado hasta que el p1 llegue.