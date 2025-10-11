## Consigna

Se desea modelar el funcionamiento de un banco en el cual existen 5 cajas para realizar
pagos. Existen P clientes que desean hacer un pago. Para esto, cada una selecciona la caja
donde hay menos personas esperando; una vez seleccionada, espera a ser atendido. En cada
caja, los clientes son atendidos por orden de llegada por los cajeros. Luego del pago, se les
entrega un comprobante. Nota: maximizar la concurrencia


>[!Important]
> Consultar, ya que me parece que la parte de el canal para avisar que se fue un cliente no me queda del todo clara
> No se que seria mejor, si solo chequear 1 canal, si tener 5 canales donde avisa cada cajero y hacer un for, sacando 1 de c/u
> o 1 while que vacie todo el canal para equiparar mejor las cosas.
```
chan espera(int)
chan esperaAviso[0..P-1](int)
chan cola[0..4](int,text)
chan comprobantes[0..P-1](text)
chan ClienteAtendido(int); // Canal para que el Cajero informe al Coordinador la liberación

process Coordinador {
    int cantPorCola[5] = ([5] 0) // indice de 0 a 4
    int cajaLiberada;
    for i 1 to N{
        int idP;
        int minimo = 0;
        int cantAux = 0;
        recieve espera(idP);
        for i 1 to 4 { // busco la cola con menor cantidad de personas
            if(cantPorCola[i] < cantAux) {
                minimo = i;
                cantAux = cantPorCola[i];
            }
        }
        cantPorCola[minimo]++;
        send esperarAviso[idP](minimo);
        while(!empty(ClienteAtendido)){ 
            receive ClienteAtendido(cajaLiberada); // Recibe el ID de la caja liberada
            cantPorCola[cajaLiberada]--;         // Resta y mantiene la coherencia
        }
        
    }
    
}

process Cliente [id: 0..P-1]{
    text comprobante;
    text pago;
    int caja;
    send espera(id);
    recieve esperaAviso[id](caja); 
    send cola[caja](id,pago); // se encola en la caja para esperar atención
    recieve comprobantes[id](comprobante); // espera el comprobante
}

process cajero[id:0..4]{
    text comprobante;
    text pago;
    int idP;
    while(true){
        recieve cola[id](idP,pago)
        // verifica el pago
        comprobante = generarComprobante(pago)
        send comprobantes[idP](comprobante)
        send ClienteAtendido[id](id);       //  NOTIFICA al Coordinador que el cliente salió
    }
}
```
