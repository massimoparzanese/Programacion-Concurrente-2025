## Consigna

Existen N vehículos que deben pasar por un puente de acuerdo con el orden de llegada.
Considere que el puente no soporta más de 50000kg y que cada vehículo cuenta con su propio
peso (ningún vehículo supera el peso soportado por el puente).


## Resolución


```
Monitor Puente {
    int peso_restante = 50000;
    Cola autos;  -- ids de vehículos en orden de llegada
    cond espera[N];

    procedure solicitarAcceso(peso, id: in int) {
        push(autos, id);

        -- esperar hasta que me toque y además haya peso
        while (id != front(autos) || peso_restante < peso) {
            wait(espera[id]);
        }

        -- ahora sí entro
        pop(autos);
        peso_restante = peso_restante - peso;
        -- despertar solo al siguiente en orden (si existe)
        if (!autos.empty()) {
            int prox = front(autos);
            signal(espera[prox]);
        }
    }

    procedure salirDelPuente(peso: in int) {
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