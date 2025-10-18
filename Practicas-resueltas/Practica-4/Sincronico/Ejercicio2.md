## Consigna

En un laboratorio de genética veterinaria hay 3 empleados. El primero de ellos
continuamente prepara las muestras de ADN; cada vez que termina, se la envía al segundo
empleado y vuelve a su trabajo. El segundo empleado toma cada muestra de ADN
preparada, arma el set de análisis que se deben realizar con ella y espera el resultado para
archivarlo. Por último, el tercer empleado se encarga de realizar el análisis y devolverle el
resultado al segundo empleado.


## Resolucion

``` falta coordinador en el medio de armador y preparador
process administrador {
    Cola Buffer;
    text muestra;
    do preparador?muestras(muestra) ->
        push(cola,muestra)
    □ not empty(Buffer); armador?listo
}
process preparador{
    text muestra;
    while(true){
        muestra = prepararMuestra();
        administrador!muestras(muestra)
    }
}
process armador {
    text m;
    set s; // set de analisis
    text r; // resultado esperado
    while (true){
        administrador?muestras(m)
        s = armarSet(m);
        analista!sets(s);
        analista?resultados(r);
    }
}
process analista{
    set s;
    text r;
    while (true){
        armador?sets(s)
        r = analizarSet(s)
        armador!resultados(r)
    }
}
```


