## Consigna

En una exposición aeronáutica hay un simulador de vuelo (que debe ser usado con
exclusión mutua) y un empleado encargado de administrar su uso. Hay P personas que
esperan a que el empleado lo deje acceder al simulador, lo usa por un rato y se retira.
a) Implemente una solución donde el empleado sólo se ocupa de garantizar la exclusión
mutua (sin importar el orden).
b) Modifique la solución anterior para que el empleado los deje acceder según el orden de
su identificador (hasta que la persona i no lo haya usado, la persona i+1 debe esperar).
c) Modifique la solución a) para que el empleado considere el orden de llegada para dar
acceso al simulador.
Nota: cada persona usa sólo una vez el simulador.