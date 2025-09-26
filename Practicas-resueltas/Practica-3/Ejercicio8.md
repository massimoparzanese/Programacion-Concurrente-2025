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
> Modificar esto para que hagan una barrera en la cual solo el ultimo de ese equipo llama a formar partido y en ese caso espera a que se le avise que esta el otro equipo. Dentro de formar partido solo 1 de ese equipo deberia esperar a que llegue el otro equipo, y en otro de los 4 montores esperan los companieros 
```
Monitor formarPartido {
    int cantEquipo[4] = [0,0,0,0]
    int equipoCancha[4]
    int canchaActual = 1
    int equiposEsperando = 0
    cond colaEquipos[4]   -- un cond por equipo
    cola esperaEquipos      -- para guardar ids de equipos completos

    procedure unirse(equipo: in int; canchaAsignada: out int){
        cantEquipo[equipo]++

        -- Si el equipo no está completo, esperar
        if cantEquipo[equipo] < 5 
            wait(colaEquipos[equipo])
        -- Si soy el último del equipo, encolar al equipo
        else if cantEquipo[equipo] == 5 {
            push(esperaEquipos, equipo)

            -- Si hay dos equipos completos, asignar cancha y avisar
            if length(esperaEquipos) >= 2{
                int eq1 = pop(esperaEquipos)
                int eq2 = pop(esperaEquipos)

                -- asignar cancha a ambos equipos
                asignarCancha(eq1, canchaActual)
                asignarCancha(eq2, canchaActual)
            }
            else {wait(colaEquipos[equipo])}     

        -- Devolver cancha asignada al jugador
        canchaAsignada = equipoCancha[equipo]
        }
    }

    procedure asignarCancha(equipo: in int, c: in int){
        -- guardar cancha asignada para el equipo
        equipoCancha[equipo] = c
        signalAll(colaEquipos[equipo]) -- despertar a todos los jugadores de ese equipo
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
    Formador.darEquipo(equipo)
    formarPartido.unirse(equipo,can)
    cancha[can].llegada()
}
```