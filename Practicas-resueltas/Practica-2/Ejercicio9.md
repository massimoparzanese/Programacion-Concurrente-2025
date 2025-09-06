## Consigna

Resolver el funcionamiento en una fábrica de ventanas con 7 empleados (4 carpinteros, 1
vidriero y 2 armadores) que trabajan de la siguiente manera:
• Los carpinteros continuamente hacen marcos (cada marco es armando por un único
carpintero) y los deja en un depósito con capacidad de almacenar 30 marcos.
• El vidriero continuamente hace vidrios y los deja en otro depósito con capacidad para
50 vidrios.
• Los armadores continuamente toman un marco y un vidrio (en ese orden) de los
depósitos correspondientes y arman la ventana (cada ventana es armada por un único
armador).

## Resolución

```
sem marcos = 30;
sem vidrios = 50;
sem fin_m = 0;
sem fin_v = 0;
sem mutexV = 1;
sem mutexM = 1;
Cola dep_marc;
Cola dep_vid;
process carpintero[id:0..3]{
    Marco m;
    while(true){
        // produce vidrio
        P(marcos)
        P(mutexM)
        push(c,m);
        V(mutexM)
        V(fin_M)
    }
}
process vidriero{
    Vidrio v;
    while(true){
        // produce vidrio
        P(vidrios)
        P(mutexV)
        push(c,v);
        V(mutexV)
        V(fin_v)
    }
}
process armador[id:0..1]{
    Marco m;
    Vidrio v;
    while(true){
        P(fin_m)
        P(mutexM)
        m = pop(dep_marc)
        V(mutexM)
        V(marcos)
        P(fin_v)
        P(mutexV)
        v = pop(dep_vid)
        V(mutexV)
        V(vidrios)
        // produce ventana
    }
}
```