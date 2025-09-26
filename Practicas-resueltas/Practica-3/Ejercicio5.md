## Consigna

En un corralón de materiales se deben atender a N clientes de acuerdo con el orden de llegada.
Cuando un cliente es llamado para ser atendido, entrega una lista con los productos que
comprará, y espera a que alguno de los empleados le entregue el comprobante de la compra
realizada.
a) Resuelva considerando que el corralón tiene un único empleado.
b) Resuelva considerando que el corralón tiene E empleados (E > 1). Los empleados no
deben terminar su ejecución.
c) Modifique la solución (b) considerando que los empleados deben terminar su ejecución
cuando se hayan atendido todos los clientes.


## Resolución


### Inciso A

```
Monitor Corralon {
    bool libre = true;
    cond esperaC;
    int esperando = 0;
    procedure llegada(idE: out int){
        if (libre) { 
            esperando ++;
            wait (esperaC);
        }
        else libre = false;
        idE = 0;
    }
    procedure proximo(){
        if (esperando > 0 ) { 
            esperando --;
            signal (esperaC);
        }
        else libre = true;
    }
}
Monitor Caja {
    cond vcCliente, vcEmpleado;
    text lista;
    text comprobante;
    // Uso de booleano para evitar deadlock, si una persona llega y hace el signal sin un empleado, el empleado se duerme y nunca se despierta
    boolean listo = false; 
    procedure atencion(papel: in text, res: out text){
        lista = papel;
        listo = true;
        signal (vcEmpleado);
        wait (vcCliente);
        // Debe esperar los resultados
        res = comprobante;
        signal (vcEmpleado); // Avisa que tomo el comprobante y se retira
    }
    procedure esperarLista(D: out text){
        if (not listo) wait (vcEmpleado);
        // Espera que llegue el cliente
        D = lista;
    }
    procedure darComprobante(R: in text){
        comprobante = R;
        signal(vcCliente);
        wait(vcEmpleado);
        listo = false;
    }
}
process cliente[id:0...N-1]{
    int idE;
    text papel, comprobante;
    Corralon.llegada(idE);
    Caja.atención(papel, comprobante);

}
process empleado {
    text datos;
    text resultado;
    while (true){
        Corralon.proximo();
        Caja.esperarLista(datos)
        // genera comprobante
        Caja.darComprobante(resultado)
    }
}
```


### Inciso B

```
Monitor Corralon {
    cola colaE;
    cond esperaC;
    int esperando = 0; // personas esperando
    int cantLibres = 0; // Empleados libres
    procedure llegada(idE: out int){
        if (cantLibres == 0) { 
            esperando ++;
            wait (esperaC);
        }
        else {
            cantLibres --;
        }
        pop(colaE,idE)
    }
    procedure proximo(idE: in int){
        push(colaE,idE)
        if (esperando > 0 ) { 
            esperando --;
            signal (esperaC);
        }
        else cantLibres ++;
        
    }
}
Monitor Caja [id:0..E-1]{
    cond vcCliente, vcEmpleado;
    text lista;
    text comprobante;
    // Uso de booleano para evitar deadlock, si una persona llega y hace el signal sin un empleado, el empleado se duerme
    // y nunca se despierta
    boolean listo = false; 
    procedure atencion(papel: in text, res: out text){
        lista = papel;
        listo = true;
        signal (vcEmpleado); // avisa al empleado que dejo el papel
        wait (vcCliente);
        // Debe esperar los resultados
        res = comprobante;
        signal (vcEmpleado); // Avisa que tomo el comprobante y se retira
    }
    procedure esperarLista(D: out text){
        if (not listo) wait (vcEmpleado); // Espera que llegue el cliente
        D = lista;
    }
    procedure darComprobante(R: in text){
        comprobante = R;
        signal(vcCliente);
        wait(vcEmpleado);
        listo = false;
    }
}
process cliente[id:0...N-1]{
    int idE;
    text papel, comprobante;
    Corralon.llegada(idE);
    Caja[idE].atención(papel, comprobante);

}
process empleado[id: 0..E-1] {
    text datos;
    text resultado;
    while (true){
        Corralon.proximo(id);
        Caja[id].esperarLista(datos)
        // genera comprobante
        Caja[id].darComprobante(resultado)
    }
}
```