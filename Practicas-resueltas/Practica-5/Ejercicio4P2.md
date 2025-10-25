## Consigna


En un sistema para acreditar carreras universitarias, hay UN Servidor que atiende pedidos
de U Usuarios de a uno a la vez y de acuerdo con el orden en que se hacen los pedidos.
Cada usuario trabaja en el documento a presentar, y luego lo envía al servidor; espera la
respuesta de este que le indica si está todo bien o hay algún error. Mientras haya algún error,
vuelve a trabajar con el documento y a enviarlo al servidor. Cuando el servidor le responde
que está todo bien, el usuario se retira. Cuando un usuario envía un pedido espera a lo sumo
2 minutos a que sea recibido por el servidor, pasado ese tiempo espera un minuto y vuelve a
intentarlo (usando el mismo documento).

## Resolución


Procedure Universidad is

    Task type usuario;

    Task servidor is
        Entry pedidos(documento: IN text, error: out boolean)
    end servidor;

    arrUsuarios: array(1..U) of usuario;


    Task body servidor is 
    begin
        loop
            Accept pedidos(documento: in text,err: out boolean) do
                err = devolverRespuesta(documento)
            end pedidos;
        end loop;
    end servidor;

    Task body usuario is
        documento: text;
        exito: boolean
    begin
        eexito:= false;
        documento = generarDocumento();
        while (not (exito)) do
            SELECT 
                Servidor.pedidos(documento, exito);
                if(not (exito)) 
                    documento = arreglarDoc();
                end if;
            Or delay 2 minutos
                delay 1 minuto
            end select;
        end while;
    end usuario;
begin
end universidad