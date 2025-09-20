## Consigna

Existen N vehículos que deben pasar por un puente de acuerdo con el orden de llegada.
Considere que el puente no soporta más de 50000kg y que cada vehículo cuenta con su propio
peso (ningún vehículo supera el peso soportado por el puente).


## Resolución


```
Monitor Puente {
    int peso_restante = 50000;
    cond cola[N];
    int siguiente = -1;
    Cola autos; // ids ordenados por orden de llegada
    process solicitarAcceso(peso,id: in int){
        if(siguiente == -1){
            siguiente = id
        }
        else{
            push(autos,id);
        }
        while(peso_restante < peso || id != siguiente){
            wait(cola[id]);
        }
        peso_restante = peso_restante - peso;
    }

    process salirDelPuente(peso: in int){
        if(!autos.empty()){
            siguiente = pop(autos);        
        }
        else{
            signal(cola[id]);
        }
        peso_restante = peso_restante + peso;
        
    }
}
process vehiculo[id:0..N-1]{
    int peso;
    Puente.solicitarAcceso(peso);
    // pasa por el puente
    Puente.salirDelPuente(peso)
}
```