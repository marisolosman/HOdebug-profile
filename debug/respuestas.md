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

SEGMENTATION FAULT

elijo el codigo fortran porque hubo un tiempo en el que use fortran

bueno, algo pasa con gfortran que no me reconoce la opcion fpp...
Me mudo a C
Pude compilar y correr small y big
small corre sin problemas
big me tira un error de violacion de segmento
Corro el codigo usando gdb (creo que tengo que tomar mas clases de gdb)
los errores que me tiran es de lectura de la varible, no puede acceder a la
memoria y la direccion de la memoria

ejecutando unlimit -s unlimited
se toma su tiempo, de hecho sigue....
mientras miro ulimit
la ayuda me dice que setea los limites del usuario, asi que ahora es ILIMITADO
me dice que es obsoleta y hay nuevos comandos

Ahora termino el codigo y no me tiro error

Ejecuto en una nueva terminal y me tira error
miro cambios en ulimit antes y despues de -s. Antes, stack size, era acotado, ahora es unlimited como otros parametros
mirando un poco que hace, creo que lo que me deja hacer es no limitar la cantidad de datos en el stack

Respecto a si esta solucion implica haber debuggeado, considero que no. Debuggear implica que mi codigo es lo suficientemente eficiente para no tener problemas de stack sin necesidad de cambiar la configuracion de stack. Lo que hice con ulimit fue decir, mi codigo es ineficiente entonces acomodo mi sistema a este codigo malo.

Me quedo mirar la carpeta bugs, valgrind y funny pero ya arranca boca

Miro Valgrind

Si lo corro con gcc opcion -O3 y miro con top la memoria. El proceso consume el 98%!!!!!!

Ahora pruebo con valgrind
si lo corro con valgrind me tira un memory error detector pero no corta

Si uso top, veo que sigue consumiendo mucho, pero ahora se de donde viene

Para saber de donde viene, tengo que compilar con la opcin -g Además, tuve que achicar el for final donde estaba la funcion mat_mul

HEAP SUMMARY:
==4887==     in use at exit: 440,000 bytes in 11 blocks
==4887==   total heap usage: 13 allocs, 2 frees, 520,000 bytes allocated
==4887== 
==4887== 440,000 bytes in 11 blocks are definitely lost in loss record 1 of 1
==4887==    at 0x4C2AB80: malloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==4887==    by 0x4005D6: mat_Tmat_mul (source1.c:30)
==4887==    by 0x4007C8: main (source1.c:59)
==4887== 
==4887== LEAK SUMMARY:
==4887==    definitely lost: 440,000 bytes in 11 blocks
==4887==    indirectly lost: 0 bytes in 0 blocks
==4887==      possibly lost: 0 bytes in 0 blocks
==4887==    still reachable: 0 bytes in 0 blocks
==4887==         suppressed: 0 bytes in 0 blocks


 

miro funny:
Si corro el codigo previamente compilado con la opcion -DDEBUG me imprime la frase IM HERE pero tira una violacion de segmento despues.
Si lo corro sin la opcion debugg no me imprime el mensaje sino que directamente me tira el error de violacion de segmento
 
