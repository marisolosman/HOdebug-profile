Tuve una reunión y no estuve en el hands-on. Va mi intento por resolver algo 
antes del partido de boca

asumo que trapfpe quiere atrapar los problemas tipo floating point exception
y no la vi en el millar de opciones de gcc pero voy a asumir que es una opcion
de este compilador. Google no me muestra algo obvio cuando busco esa opcion

Cuando compilo sin esa opcion, todos compilan bien menos el 3 que me tira un warning de incompatibilidad de una funcion declarada implicitamente sqrtf que fue 
habilitada por default
Pero cuando linkeo gcc codigo.o -o codigo.e pasa lo siguiente al correr

El caso 1: ingreso a,b 1,2 y me devuelve 0.5

El caso 2: ingreso a,b 1,2 y me devuelve -1 (es lo que espera el codigo)

El caso 3: ingreso a,b 1,2 y me devuelve error
´´´ referencia a sqrtf sin definir 
collect2: error: ld returned 1 exit status ´´´

Mirando el trabajo de mi compañera carla (gracias github) pude ver que ella
probo la funcion 1 con varias opciones y, segun la opcion, el codigo devuelve
fruta en la division que hace en 0/0 (daba 0)
Ella menciona que hay que incluir la opcion trapfpe. Voy a probar si entiendo
las sentencias que ella pone...
ejecuto gcc -DTRAPFPE -Ifpe_x87_sse -c test_fpe1.c -o test_fpe1.o
genera el .o
no habia compilado el codigo fpe, lo hago porque me tiro error al hacer
gcc ./fpe_x87_sse/fpe_x87_sse.o test_fpe1.o -lm -o test_fpe1.e

Ademas de linkear a fpe... linkeo a m porque lo necesita fpe
Pruebo la funcion con 0/0 y me devuelte un fpe (core generado)
esta bien porque esa cuenta explota
Siguiendo con la misma logica para los otros dos codigos...
Ahora corre bien y pasa lo mismo que con el caso 1
El test 3 no tiene problemas porque esta linkeado a m

