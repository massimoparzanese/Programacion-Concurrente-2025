## Consigna

Desarrolle una solución de grano fino usando sólo variables compartidas (no se puede usar
las sentencias await ni funciones especiales como TS o FA). En base a lo visto en la clase 3
de teoría, resuelva el problema de acceso a sección crítica usando un proceso coordinador.
En este caso, cuando un proceso SC[i] quiere entrar a su sección crítica le avisa al coordinador,
y espera a que éste le dé permiso. Al terminar de ejecutar su sección crítica, el proceso SC[i]
le avisa al coordinador. Nota: puede basarse en la solución para implementar barreras con
“Flags y coordinador” vista en la teoría 3.

## Respuesta

```
int arribo[1:n] = ([n] 0), continuar[1:n] = ([n] 0);
bool termine = false;
process Worker[i=1 to n]
{ while (true)
    { arribo[i] = 1;
      while (continuar[i] == 0) skip;
      // realiza su tarea
      continuar[i] = 0;
      termine = true;
    }
}
process Coordinador
{ while (true)
    { for [i = 1 to n]
        { if(arribo[i] == 1){
            arribo[i] = 0
            continuar[i] = 1;
            while (termine == false) skip;
            termine = false;
        }
    }
    }
}
```