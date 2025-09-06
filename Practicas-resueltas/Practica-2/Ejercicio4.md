## Consigna

Suponga que existe una BD que puede ser accedida por 6 usuarios como máximo al
mismo tiempo. Además, los usuarios se clasifican como usuarios de prioridad alta y
usuarios de prioridad baja. Por último, la BD tiene la siguiente restricción:
• no puede haber más de 4 usuarios con prioridad alta al mismo tiempo usando la BD.
• no puede haber más de 5 usuarios con prioridad baja al mismo tiempo usando la BD.
Indique si la solución presentada es la más adecuada. Justifique la respuesta.

```
Var
total: sem := 6;
alta: sem := 4;
baja: sem := 5;

Process Usuario-Alta [I:1..L]::
{ 
P (total);
P (alta);
//usa la BD
V(total);
V(alta);
}

Process Usuario-Baja [I:1..K]::
{ 
P (total);
P (baja);
//usa la BD
V(total);
V(baja);
}
```

## Resolución

No es la más adecuada, ya que si entran 6 de prioridad alta, total va a ser = 0 y va a haber 2 esperando a acceder a la bd, y otros procesos de menor prioridad que podrían acceder tranquilamente a la bd sin necesidad de demorarse de forma innecesaria
