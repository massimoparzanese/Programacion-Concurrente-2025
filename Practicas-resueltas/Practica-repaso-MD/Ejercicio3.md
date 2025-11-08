## Cosigna

Resolver el siguiente problema con PMA. En un negocio de cobros digitales hay P personas que
deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son
atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos
de 5 boletas de los que pagan más. Adicionalmente, las personas embarazadas tienen prioridad sobre
los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les
devuelve el vuelto y los recibos de pago.

## Resolución

> genera busy waiting pero no se me ocurrio otra manera sin hacerlo y sin generar que se quede esperando en un canal que puede no tener mas gente
```
chan recibosMayor(int,int,int);
chan recibosMenor(int,int,int);
chan recibosAncianos(int,int,int);
chan recibos[P](text,int);
chan avisos();
process caja {
    vuelto: real;
    recibo: text;
    idP: int;
    int dinero: real;
    cantBoletas: int;
    habia: bool;
    while (true){ 
        recieve avisos();
        habia = false;
        if(not empty(recibosAncianos)) {
            recieve recibosAncianos(cantBoletas,dinero,idP)
        }
        else if (not empty (recibosMenor)){
            recieve recibosMenor(cantBOletas,dinero,idP);
        }
        else {
            recieve recibosMayor(cantBoletas,dinero,idP)
        }
        
        vuelto = cobrarBoletas(cantBoletas,dinero);
        recibo = generarRecibo(cantBoletas,dinero, vuelto);
        send recibos[idP](recibo,vuelto);
        
        
    }
}

process persona[id:0..P-1]{
    cantBoletas: int;
    dinero: real;
    recibo: text;
    vuelto: real;
    anciano: bool;
    anciano = ...; // si soy anciano esta en true
    cantBoletas =...; // cant definida
    dinero = ..;
    if(anciano){
        send reciboAncianos(cantBoletas,dinero,id)
    }
    else if (cantBoletas < 5) {
        send recibosMenor(cantBoletas,dinero,id)
    }
    else {
        send recibosMayor(cantBoletas,dinero,id)
    }
    recieve recibos[id](recibo, vuelto);
}
```