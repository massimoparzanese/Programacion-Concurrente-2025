## Consigna

En una clínica existe un médico de guardia que recibe continuamente peticiones de
atención de las E enfermeras que trabajan en su piso y de las P personas que llegan a la
clínica ser atendidos.
Cuando una persona necesita que la atiendan espera a lo sumo 5 minutos a que el médico lo
haga, si pasado ese tiempo no lo hace, espera 10 minutos y vuelve a requerir la atención del
médico. Si no es atendida tres veces, se enoja y se retira de la clínica.
Cuando una enfermera requiere la atención del médico, si este no lo atiende inmediatamente
le hace una nota y se la deja en el consultorio para que esta resuelva su pedido en el
momento que pueda (el pedido puede ser que el médico le firme algún papel). Cuando la
petición ha sido recibida por el médico o la nota ha sido dejada en el escritorio, continúa
trabajando y haciendo más peticiones.
El médico atiende los pedidos dándole prioridad a los enfermos que llegan para ser atendidos.
Cuando atiende un pedido, recibe la solicitud y la procesa durante un cierto tiempo. Cuando
está libre aprovecha a procesar las notas dejadas por las enfermeras.




## Resolución 

```ada
Procedure clinica is

    Task medico is
        Entry pedidosCli(pedido: in text, resp: out text);
        Entry pedidosEnf(pedido: in text);
    end medico;
    Task type cliente;
    Task type enfermera;
    Task escritorio is
        Entry pedidosEnf(pedido: in text);
    end escritorio;

    arrCLiente: array(1..P) of cliente;
    arrEnfermera: array(1..E) of enfermera;

    Task body cliente is
        id: int;
        solicitud: text;
        intentos: int;
        atendido: Boolean;
        resp: text
    begin
        intentos:= 0;
        atendido:= false;
        while(not atendido and intentos < 3) // Consultar esta estructura, que se puede hacer, no se si existe algo asi
            SELECT 
                Medico.pedidosCLi(solicitud,resp);
                atendido:= true;
            OR delay 5 minutos
                intentos = intentos + 1;
                delay 10 minutos
            end select;
        end while;
    end cliente;


    Task body enfermera is
        text: pedido;
    begin
        LOOP
            SELECT 
                Medico.pedidosEnf(pedido: in text)
            ELSE
                Escritorio.pedidosEnf(pedido: in text);
            end select;
        END loop;
    end enfermera;

    Task body escritorio is
        pedido: text;
        pedidos: cola; // Consultar tema de uso de cola para no hacer esperar enfermeras
    begin
        loop
            SELECT 
                ACCEPT pedidosEnf(pedido: in text) do
                    push(cola,pedido)
                end pedidosEnf;
            OR
                Medico.pedidosEnf(pop(cola))
            end select;
        end loop;
    end escritorio;

    Task body medico is
        p: text;
    begin
        LOOP
            SELECT 
                ACCEPT pedidosCli(pedido: in text, resp: out text) do
                    p = pedido;
                    resp = atenter(p);
                end pedidosCli;
            Else 
                Accept pedidosEnf(pedido: in text);
            end Select;
        END loop;
    end medico;
begin
end clinica;
```