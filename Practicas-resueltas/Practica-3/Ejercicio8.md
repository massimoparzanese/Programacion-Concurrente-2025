## Consigna

En un entrenamiento de fútbol hay 20 jugadores que forman 4 equipos (cada jugador conoce
el equipo al cual pertenece llamando a la función DarEquipo()). Cuando un equipo está listo
(han llegado los 5 jugadores que lo componen), debe enfrentarse a otro equipo que también
esté listo (los dos primeros equipos en juntarse juegan en la cancha 1, y los otros dos equipos
juegan en la cancha 2). Una vez que el equipo conoce la cancha en la que juega, sus jugadores
se dirigen a ella. Cuando los 10 jugadores del partido llegaron a la cancha comienza el partido,
juegan durante 50 minutos, y al terminar todos los jugadores del partido se retiran (no es
necesario que se esperen para salir).

## Resolucion

> [!important]
> Consultar, ya se le realizaron las modificaciones pedidas.
```
Monitor formarPartido {
    int equipoCancha[4]
    int canchaActual = 1
    int equiposEsperando = 0
    cond colaEquipos[4]   -- un cond por equipo
    cola esperaEquipos    -- para guardar ids de equipos completos

    procedure unirse(equipo: in int; canchaAsignada: out int){
            push(esperaEquipos, equipo);
            if length(esperaEquipos) >= 2{
                int eq1 = pop(esperaEquipos)
                int eq2 = pop(esperaEquipos)
                equipoCancha[eq1] = canchaActual;
                equipoCancha[eq2] = canchaActual;
                canchaActual ++;
                signal(colaEquipos[eq1])
            }
            else {wait(colaEquipos[equipo])}     

        -- Devolver cancha asignada al jugador
        canchaAsignada = equipoCancha[equipo]
    }
}
Monitor barreraEquipos[id:0..4]{
    int cantEquipo = 0;
    cond compañeros;
    int c;
    prodecure listo(cancha: out int){
        cantEquipo ++;
        if(cant < 5) wait(compañeros)
        else {
            formarPartidos.unirse(id,c); // podria llamarse desde fuera del monitor y que el signal all lo haga desde otro procedure para no bloquear el monitor
            signal_all(compañeros)
        }
        cancha = c;
    }
    
}
Monitor cancha[id:0..1]{
    int jugadores = 0;
    cond esperar
    cond terminar;

    procedure llegada(){
        cant ++;
        if(cant == 10) signal(esperar)
        wait(terminar) 
    }
    procedure iniciar(){
        if(jugadores < 10) wait(esperar)
    }
    procedure terminar(){
        signal_all(terminar)
    }

}
Monitor Formador {

    procedure darEquipo(idE: out int){
        idE = DarEquipo()
    }
}
process partido[id:0..1]{
    cancha.iniciar()
    delay(50 minutos)
    cancha.terminar();
}
process jugador[id:0..19]{
    int equipo;
    int can;
    Formador.darEquipo(equipo);
    barreraEquipos[equipo].listo(can);
    cancha[can].llegada()
}
```