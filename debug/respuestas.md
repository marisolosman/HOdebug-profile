Tuve una reunión y no estuve en el hands-on. Va mi intento por resolver algo 
antes del partido de boca

asumo que trapfpe quiere atrapar los problemas tipo floating point exception
y no la vi en el millar de opciones de gcc pero voy a asumir que es una opcion
de este compilador. Google no me muestra algo obvio cuando busco esa opcion

Cuando compilo sin esa opcion, todos compilan bien menos el 3 que me tira un warning de incompatibilidad de una funcion declarada implicitamente sqrtf que fue 
habilitada por default
Pero cuando linkeo gcc codigo.o -o codigo.e pasa lo siguiente al correr

El caso 1: ingreso a,b 1,2 y me devuelve 0.5

El caso 2: ingreso a,b 1,2 y me devuelve -1

El caso 3: ingreso a,b 1,2 y me devuelve error
´´´ referencia a sqrtf sin definir 
collect2: error: ld returned 1 exit status ´´´



