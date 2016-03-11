Hands On profile

Compilo los codigos en C con diferentes opciones
Uso variantes de profiler

Opcion -O0

Si lo miro con time

´´´ Violación de segmento (core generado)

real	0m0.104s
user	0m0.000s
sys	0m0.001s

´´´

Opcion -O1 (default)

Miro con time

Tengo violacion de segmento

real	0m0.110s
user	0m0.000s
sys	0m0.002s

Tarda mas

Opcion -O3
No tengo violacion de segmento

real	0m0.006s
user	0m0.000s
sys	0m0.001s

ESta optimizacion "resuelve" mi bug

Mirando con gprof

solo me genera un archivo gmon cuando corro con opcion -O3
las opciones 0 y 1 no generan gmon


Miro con perf la compilacion con opcpion -O0

./out1.e: Violación de segmento

 Performance counter stats for './out1.e':

          0,675440 task-clock (msec)         #    0,007 CPUs utilized          
                 3 context-switches          #    0,004 M/sec                  
                 2 cpu-migrations            #    0,003 M/sec                  
               116 page-faults               #    0,172 M/sec                  
         1.044.696 cycles                    #    1,547 GHz                    
           660.266 stalled-cycles-frontend   #   63,20% frontend cycles idle   
           513.103 stalled-cycles-backend    #   49,12% backend  cycles idle   
           803.251 instructions              #    0,77  insns per cycle        
                                             #    0,82  stalled cycles per insn
           152.488 branches                  #  225,761 M/sec                  
             7.092 branch-misses             #    4,65% of all branches        

       0,099733133 seconds time elapsed

Probando compilando con -O3


0,900737 task-clock (msec)         #    0,120 CPUs utilized          
                 8 context-switches          #    0,009 M/sec                  
                 1 cpu-migrations            #    0,001 M/sec                  
               138 page-faults               #    0,153 M/sec                  
         1.379.305 cycles                    #    1,531 GHz                    
           927.647 stalled-cycles-frontend   #   67,25% frontend cycles idle   
           765.283 stalled-cycles-backend    #   55,48% backend  cycles idle   
           877.210 instructions              #    0,64  insns per cycle        
                                             #    1,06  stalled cycles per insn
           169.944 branches                  #  188,672 M/sec                  
            11.353 branch-misses             #    6,68% of all branches        

       0,007525442 seconds time elapsed

Miro ahora los diferentes resultados con perf

Primero tengo que resolver el bug. No lo hice, sino que le di memoria ilimitada para que no me tire error de segmentation

mirando el registro que genero con perf:
tengo el % que representa cada proceso siendo la primera asignacion la de mayor porcentaje


Miro el profile 2

El problema esta cuando defino dim porque necesita imput del usuario y si no pongo un valor me va a apuntar a cualquier lado y despues explota al generar variables de dimension dim
