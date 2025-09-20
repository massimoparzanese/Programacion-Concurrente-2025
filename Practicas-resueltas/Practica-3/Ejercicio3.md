## Consigna

Existen N personas que deben fotocopiar un documento. La fotocopiadora sólo puede ser
usada por una persona a la vez. Analice el problema y defina qué procesos, recursos y
monitores serán necesarios/convenientes, además de las posibles sincronizaciones requeridas
para resolver el problema. Luego, resuelva considerando las siguientes situaciones:
a) Implemente una solución suponiendo no importa el orden de uso. Existe una función
Fotocopiar() que simula el uso de la fotocopiadora.
b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.
c) Modifique la solución de (b) para el caso en que se deba dar prioridad de acuerdo con la
edad de cada persona (cuando la fotocopiadora está libre la debe usar la persona de mayor
edad entre las que estén esperando para usarla).
d) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden
dado por el identificador del proceso (la persona X no puede usar la fotocopiadora hasta
que no haya terminado de usarla la persona X-1).
e) Modifique la solución de (b) para el caso en que además haya un Empleado que le indica
a cada persona cuando debe usar la fotocopiadora.
f) Modificar la solución (e) para el caso en que sean 10 fotocopiadoras. El empleado le indica
a la persona cuál fotocopiadora usar y cuándo hacerlo.

## Resolución

### Inciso A

```
Monitor Fotocopiadora{
    Procedure solicitarFotocopiadora () { 
        Fotocopiar();
    }
}
process persona[id:0..N-1]{
    Fotocopiadora.solicitarFotocopiadora()
}
```

### Inciso B

```
Monitor Fotocopiadora{
    bool libre = true;
    cond cola;
    int esperando = 0;
    Procedure solicitarFotocopiadora (){ 
        if (not libre) { 
            esperando ++;
            wait(cola);
        }
        else libre = false;
    }
    Procedure liberarFotocopiadora () { 
        if (esperando > 0 ) { 
            esperando --;
            signal (cola);
        }
        else libre = true;
    }
}
process persona[id:0..N-1]{
    Fotocopiadora.solicitarFotocopiadora();
    Fotocopiar();
    Fotocopiadora.liberarFotocopiadora();
}
```

### Inciso C

```
Monitor Fotocopiadora{
    bool libre = true;
    cond espera[N];
    int idAux, esperando = 0; 
    colaOrdenada fila;
    Procedure solicitarFotocopiadora (idP,edad: in int){ 
        if (not libre) { 
            insertar(fila, idP, edad);
            esperando ++;
            wait (espera[idP]);
        }
        else libre = false;
    }
    Procedure liberarFotocopiadora () { 
        if (esperando > 0 ) { 
            esperando --;
            sacar (fila, idAux);
            signal (espera[idAux]);
        }
        else libre = true;
    }
}
process persona[id:0..N-1]{
    int edad;
    Fotocopiadora.solicitarFotocopiadora(id, edad);
    Fotocopiar();
    Fotocopiadora.liberarFotocopiadora();
}

```

### Inciso D

```
Monitor Fotocopiadora{
    cond espera[N];
    colaOrdenada fila;
    siguiente = 0;
    Procedure solicitarFotocopiadora (idP: in int){ 
        if (siguiente != idP) { 
            wait (espera[idP]);
        }
    }
    Procedure liberarFotocopiadora () { 
        siguiente ++;
        signal (espera[siguiente]);
    }
}
process persona[id:0..N-1]{
    Fotocopiadora.solicitarFotocopiadora(id);
    Fotocopiar();
    Fotocopiadora.liberarFotocopiadora();
}

```

### Inciso E

```
Monitor Fotocopiadora {
    int esperando = 0;
    cond esperaC;
    con esperaE;
    con esperaFot;
    Procedure Llegada(){ 
        esperando ++;
        signal(esperaE);
        wait (esperaC);
    }
    Procedure Próximo(){ 
        if(esperando == 0) wait(esperaE);
        esperando --;
        signal(esperaC); 
        wait(esperaFot)
    }
    Procedure LiberarFotocopiadora(){
        signal(esperaFot);
    }
}
process persona[id:0..N-1]{
    Fotocopiadora.Llegada()
    Fotocopiar();
    Fotocopiadora.LiberarFotocopiadora();
}
process empleado {
    while(true){
        Fotocopiadora.Proximo();
    }
} 
```