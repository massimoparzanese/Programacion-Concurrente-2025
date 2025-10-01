## Consigna

Resolver el siguiente problema. En una montaña hay 30 escaladores que en una parte de la subida
deben utilizar un único paso de a uno a la vez y de acuerdo con el orden de llegada al mismo. Nota:
sólo se pueden utilizar procesos que representen a los escaladores; cada escalador usa sólo una vez
el paso.

## Resolución

```
Monitor Admin{
    cond espera;
    int esperando = 0;
    bool libre = true
    procedure llegar(){
        if(libre){
            libre = false;
        }
        else {
            esperando ++;
            wait(espera);
            esperando --;
        }
    }

    procedure salir(){
        if(esperando == 0) libre = true;
        else signal(espera);
    }
}

procedure escalador[id:0..29]{
    Admin.llegar()
    //escala
    Admin.salir()
}
```