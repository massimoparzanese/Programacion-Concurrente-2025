## Consigna

Se dispone de un sistema compuesto por 1 central y 2 procesos periféricos, que se
comunican continuamente. Se requiere modelar su funcionamiento considerando las
siguientes condiciones:
- La central siempre comienza su ejecución tomando una señal del proceso 1; luego
toma aleatoriamente señales de cualquiera de los dos indefinidamente. Al recibir una
señal de proceso 2, recibe señales del mismo proceso durante 3 minutos.
- Los procesos periféricos envían señales continuamente a la central. La señal del
proceso 1 será considerada vieja (se deshecha) si en 2 minutos no fue recibida. Si la
señal del proceso 2 no puede ser recibida inmediatamente, entonces espera 1 minuto y
vuelve a mandarla (no se deshecha).

## Resolución

```ada
Process sistemaComputo is
    
    Task central is
        Entry pedidos1(señal: in text);
        Entry pedidos2(señal: in text);
    end central;

    Task periferico1;
    Task periferico2;

    Task body periferico1 is
        señal: text;
    begin
        LOOP
            señal = ..;
            SELECT 
                Central.pedidos1(señal: in text);
            OR DELAY 2 minutos
                // descarta señal
            end select;
        END LOOP;
    end periferico1;

    Task body periferico2 is
        señal: text;
    begin
        señal = ..;
        LOOP
            SELECT 
                Central.pedidos1(señal:in text);
                señal = ..;
            ELSE 
                delay 1 minuto
            end select;
        END LOOP;
    end periferico2;


    Task body central is
        señal: text;
        exclusivo: Boolean
    begin
        exclusivo:= false;
        ACCEPT pedidos1(señal:in text);
        LOOP
            if(exclusivo) then
                SELECT
                    accept pedidos1(señal:in text);
                or
                    accept pedidos2(señal:in text);
                    modo_exclusivo := True;
                end select;
            else
                SELECT
                    accept pedidos2(señal: in text);
                or delay 3 minutos
                    modo_exclusivo := False;
                end select;
            end if;
        END loop;
    end;

begin
    null;
end sistemaComputo;
```