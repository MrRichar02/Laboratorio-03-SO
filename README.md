# <img width="632" height="369" alt="image" src="https://github.com/user-attachments/assets/4262d988-f49a-494f-beac-357f66259147" />


**Laboratorio de Sistemas Operativos Gestión de Memoria**  
**Practica No. 3**

**Ricardo Medina Herrera**  
**Santiago Villegas Naranjo**

# Espacio de direcciones

## 1.1 Actividad: Programa base

Se nos da el siguiente archivo 

```c
// mem_map.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int global_var = 42; /* segmento de datos (.data) */

int main() {
  int local_var = 10;                        /* stack */
  int *heap_var = malloc(sizeof(int) * 100); /* heap */
  *heap_var = 99;

  printf("PID del proceso : %d\n", getpid());
  printf("Dir. codigo (main) : %p\n", (void *)main);
  printf("Dir. global_var : %p\n", (void *)&global_var);
  printf("Dir. local_var : %p\n", (void *)&local_var);
  printf("Dir. heap_var : %p\n", (void *)heap_var);

  printf("\nPresione ENTER para continuar...\n");
  getchar();
  free(heap_var);
  return 0;
}
```

Se compilo el archivo a un ejecutable con el mismo nombre del archivo pero sin la extensión

## 1.2 Actividad: Visualizar los mapas de memoria de un proceso

Se ejecutó el ejecutable del programa mem\_map y luego se ejecutaron los comandos detallados en la guía obteniendo el siguiente resultado

Ejecución del programa

Salida

```
./mem_map
PID del proceso : 13271
Dir. codigo (main) : 0x5642404080b0
Dir. global_var : 0x56424040b010
Dir. local_var : 0x7ffe3104e6a4
Dir. heap_var : 0x56427c0922a0

Presione ENTER para continuar...
```

Comando ejecutado:

```
cat /proc/$(pgrep mem_map)/maps
```

Salida del comando

```
 cat /proc/$(pgrep mem_map)/maps
564240407000-564240408000 r--p 00000000 00:1a 6262837                    /home/docair/Documents/octavo-semestre/so/labs/tercero/1-espacio-direcciones/mem_map
564240408000-564240409000 r-xp 00001000 00:1a 6262837                    /home/docair/Documents/octavo-semestre/so/labs/tercero/1-espacio-direcciones/mem_map
564240409000-56424040a000 r--p 00002000 00:1a 6262837                    /home/docair/Documents/octavo-semestre/so/labs/tercero/1-espacio-direcciones/mem_map
56424040a000-56424040b000 r--p 00002000 00:1a 6262837                    /home/docair/Documents/octavo-semestre/so/labs/tercero/1-espacio-direcciones/mem_map
56424040b000-56424040c000 rw-p 00003000 00:1a 6262837                    /home/docair/Documents/octavo-semestre/so/labs/tercero/1-espacio-direcciones/mem_map
56427c092000-56427c0b3000 rw-p 00000000 00:00 0                          [heap]
7fae20c00000-7fae20c28000 r--p 00000000 00:1a 1580057                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/libc.so.6
7fae20c28000-7fae20da6000 r-xp 00028000 00:1a 1580057                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/libc.so.6
7fae20da6000-7fae20df5000 r--p 001a6000 00:1a 1580057                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/libc.so.6
7fae20df5000-7fae20df9000 r--p 001f4000 00:1a 1580057                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/libc.so.6
7fae20df9000-7fae20dfb000 rw-p 001f8000 00:1a 1580057                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/libc.so.6
7fae20dfb000-7fae20e08000 rw-p 00000000 00:00 0
7fae20ecf000-7fae20ed4000 rw-p 00000000 00:00 0
7fae20ed4000-7fae20ed8000 r--p 00000000 00:00 0                          [vvar]
7fae20ed8000-7fae20eda000 r-xp 00000000 00:00 0                          [vdso]
7fae20eda000-7fae20edb000 r--p 00000000 00:1a 1580051                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/ld-linux-x86-64.so.2
7fae20edb000-7fae20f05000 r-xp 00001000 00:1a 1580051                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/ld-linux-x86-64.so.2
7fae20f05000-7fae20f0f000 r--p 0002b000 00:1a 1580051                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/ld-linux-x86-64.so.2
7fae20f0f000-7fae20f11000 r--p 00035000 00:1a 1580051                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/ld-linux-x86-64.so.2
7fae20f11000-7fae20f13000 rw-p 00037000 00:1a 1580051                    /nix/store/vr7ds8vwbl2fz7pr221d5y0f8n9a5wda-glibc-2.40-218/lib/ld-linux-x86-64.so.2
7ffe31030000-7ffe31054000 rw-p 00000000 00:00 0                          [stack]
```

Comando ejecutado

```
pmap -x $(pgrep mem_map)
```

Salida del comando

```
13271:   ./mem_map
Address           Kbytes     RSS   Dirty Mode  Mapping
0000564240407000       4       4       0 r---- mem_map
0000564240408000       4       4       0 r-x-- mem_map
0000564240409000       4       4       0 r---- mem_map
000056424040a000       4       4       4 r---- mem_map
000056424040b000       4       4       4 rw--- mem_map
000056427c092000     132       4       4 rw---   [ anon ]
00007fae20c00000     160     156       0 r---- libc.so.6
00007fae20c28000    1528    1056       0 r-x-- libc.so.6
00007fae20da6000     316     128       0 r---- libc.so.6
00007fae20df5000      16      16      16 r---- libc.so.6
00007fae20df9000       8       8       8 rw--- libc.so.6
00007fae20dfb000      52      20      20 rw---   [ anon ]
00007fae20ecf000      20      16      16 rw---   [ anon ]
00007fae20ed4000      16       0       0 r----   [ anon ]
00007fae20ed8000       8       4       0 r-x--   [ anon ]
00007fae20eda000       4       4       0 r---- ld-linux-x86-64.so.2
00007fae20edb000     168     168       0 r-x-- ld-linux-x86-64.so.2
00007fae20f05000      40      40       0 r---- ld-linux-x86-64.so.2
00007fae20f0f000       8       8       8 r---- ld-linux-x86-64.so.2
00007fae20f11000       8       8       8 rw--- ld-linux-x86-64.so.2
00007ffe31030000     144      24      24 rw---   [ stack ]
---------------- ------- ------- -------
total kB            2648    1680     112<
```

## 1.3 Actividad: Exploración de /proc/\[pid\]/maps

1\. Identifique en la salida de /proc/maps las regiones text, heap y stack. ¿Qué permisos (r/w/x/p) tiene cada una? ¿Por qué difieren?

Región text: Tiene permisos de lectura y ejecución

```
564240408000-564240409000 r-xp 00001000 00:1a 6262837                    /home/docair/Documents/octavo-semestre/so/labs/tercero/1-espacio-direcciones/mem_map
```

La región text contiene el código ejecutable del programa, incluyendo la función main. Tiene permisos r-xp porque el código debe poder leerse y ejecutarse, pero no modificarse durante la ejecución del proceso.

Región heap: Tiene permisos de lectura y escritura

```
56427c092000-56427c0b3000 rw-p 00000000 00:00 0                          [heap]
```

La región heap almacena memoria dinámica reservada mediante malloc(), por lo que necesita permisos de lectura y escritura. No posee permisos de ejecución porque allí no se ejecuta código.

Región stack: Tiene permisos de lectura y escritura

```
7ffe31030000-7ffe31054000 rw-p 00000000 00:00 0                          [stack]
```

La región stack almacena variables locales, parámetros y llamadas a funciones. Tiene permisos de lectura y escritura porque su contenido cambia constantemente durante la ejecución del programa. Tampoco posee permisos de ejecución debido a que no está destinada para ejecutar instrucciones.

**Nota:** La región de text no tiene permisos de escritura porque es un binario y no tiene sentido que se pueda modificar el binario, las regiones de heap y stack no tienen el permiso de ejecución porque 

2\. Compare las direcciones impresas con los rangos de /proc/maps. ¿A qué región pertenece  
cada variable?

Las direcciones impresas por el programa corresponden a distintas regiones del espacio de memoria virtual del proceso.

**La función main tiene dirección:**

0x5642404080b0

y pertenece a la región text del ejecutable, ya que coincide con el rango ejecutable del programa mostrado en /proc/maps.

**La variable global\_var tiene dirección:**

0x56424040b010

y pertenece al segmento de datos (.data), correspondiente a una región con permisos de lectura y escritura del ejecutable.

**La variable local\_var tiene dirección:**

0x7ffe3104e6a4

y pertenece a la región stack, debido a que es una variable local declarada dentro de la función main.

**La variable heap\_var tiene dirección:**

0x56427c0922a0

y pertenece a la región heap, ya que fue reservada dinámicamente mediante malloc().

3\. ¿Qué otras regiones aparecen en el mapa (\`libc\`, \`\[vdso\]\`, \`\[vsyscall\]\`)? ¿Qué función cumple cada una?

- libc: En esta región se encuentra la biblioteca estándar de C, la librería de C GNU, aquí podemos encontrar funciones esenciales como printf, malloc, free entre otros  
- ld-linux-x86-64.so.2: Es el dynamic linker / loader de Linux, será el primero en ser llamado por el sistema operativo, su trabajo será obtener las bibliotecas y resolver los símbolos dinámicos antes de ejecutar el main, por ejemplo al ver que nuestro programa usa funciones que dependen de libc se encargará de cargarlo  
- Virtual Dynamic Shared Object(vdso): Los llamados al sistema son costosos por operaciones que implican como cambios parciales de contexto, entonces para hacer llamados de sistema para funciones más simples se utiliza esta región, una operación de ejemplo sería obtener el tiempo de esta forma se tiene un mejor rendimiento  
- Virtual variables(vvar): Esta región complementa a vdso porque en esta el sistema operativo compartirá información el proceso como el tiempo actual, para que luego cuando el proceso solicite esa información con un llamado al sistema en lugar de hacer un llamado al sistema la región vdso usa esta para obtener la información  
- anon: Es una región similar al heap pero aqui se almacenan las regiones creadas utilizando mmap a diferencia que en el heap donde se guarda lo asociado al malloc  
- vsyscall: no aparecio en la ejecución de los comandos 

4\. ¿Son las direcciones virtuales iguales a las físicas? Explique apoyándose en el concepto de address space del OSTEP.

No, las direcciones observadas corresponden a direcciones virtuales y no a direcciones físicas reales de la memoria RAM. Según el concepto de address space explicado en OSTEP, cada proceso posee su propio espacio de direcciones virtual privado y aislado. El sistema operativo crea la ilusión de que cada proceso tiene memoria continua y única.

Internamente, la memory management unit y las tablas de páginas traducen las direcciones virtuales utilizadas por el proceso hacia direcciones físicas reales en memoria. Gracias a este mecanismo, diferentes procesos pueden utilizar espacios virtuales sin interferir entre sí.       

## 1.4 Actividad: Comparar espacios de dos procesos simultáneos

Primera instancia del programa

```
PID del proceso : 7082
Dir. codigo (main) : 0x557a45608199
Dir. global_var : 0x557a4560b048
Dir. local_var : 0x7ffe6803aaac
Dir. heap_var : 0x557a4acf92a0

Presione ENTER para continuar...
```

Segunda instancia del programa 

```
PID del proceso : 8332
Dir. codigo (main) : 0x5644e72c3199
Dir. global_var : 0x5644e72c6048
Dir. local_var : 0x7ffc08a48aec
Dir. heap_var : 0x56450228a2a0

Presione ENTER para continuar...

```

1. ¿Son las mismas direcciones virtuales en ambos procesos? ¿Qué conclusión saca sobre el aislamiento del espacio de direcciones?

   No. Aunque ambos ejecutan el mismo programa, cada proceso posee su propio espacio de direcciones virtuales independiente. El sistema operativo, mediante la MMU y el proceso de traducción, traduce las direcciones virtuales de cada proceso a distintas direcciones físicas de memoria. Esto garantiza el aislamiento entre procesos, evitando que un proceso pueda acceder directamente a la memoria de otro.

2. ¿Podría el Proceso A leer o modificar la variable global del Proceso B mediante su dirección virtual? Justifique.  
     
   No, el proceso A no puede acceder directamente a la variable global del proceso B usando su dirección virtual. Ya que las direcciones virtuales solamente tienen validez dentro del espacio de direcciones del proceso propietario, aunque ambos procesos tengan variables similares, el sistema operativo mantiene aislamiento y protección entre procesos mediante memoria virtual y tablas de páginas.   
     
   Por esta razón, una dirección virtual perteneciente al proceso B no puede ser utilizada directamente por el proceso A para leer o modificar memoria de otro proceso. 

# API de Memoria

## 2.1 Actividad: Programa base

Comando ejecutado 

```
gcc -Wall -o heap_demo heap_demo.c
```

El comando no produjo una salida

Comando ejecutado

```
valgrind --leak-check=full --track-origins=yes ./heap_demo
```

Salida del comando 

```
==17108== Memcheck, a memory error detector
==17108== Copyright (C) 2002-2024, and GNU GPL'd, by Julian Seward et al.
==17108== Using Valgrind-3.26.0 and LibVEX; rerun with -h for copyright info
==17108== Command: ./heap_demo
==17108==
Arreglo original: 0 1 4 9 16 25 36 49 64 81
Arreglo ampliado: 0 1 4 9 16 25 36 49 64 81 100 121 144 169 196 225 256 289 324 361
==17108==
==17108== HEAP SUMMARY:
==17108==     in use at exit: 0 bytes in 0 blocks
==17108==   total heap usage: 3 allocs, 3 frees, 1,144 bytes allocated
==17108==
==17108== All heap blocks were freed -- no leaks are possible
==17108==
==17108== For lists of detected and suppressed errors, rerun with: -s
==17108== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

## 2.2 Actividad: Uso correcto de malloc y free

1. Muestre la salida completa de Valgrind. ¿Reporta errores o fugas de memoria? ¿Qué significa el mensaje "All heap blocks were freed"?

	  
	La ejecución de Valgrind sobre heap\_demo no reportó errores ni fugas de memoria.  
		  
	**El resumen mostrado fue:**    
	  
	

```
HEAP SUMMARY:
in use at exit: 0 bytes in 0 blocks
total heap usage: 3 allocs, 3 frees, 1,144 bytes allocated

All heap blocks were freed -- no leaks are possible

ERROR SUMMARY: 0 errors from 0 contexts
```

Esto significa que toda la memoria reservada dinámicamente mediante malloc() y realloc() fue  
correctamente liberada utilizado free(), por lo que no quedaron bloques de memoria ocupados al finalizar el programa.

El mensaje “All heap blocks were freed \- \- no leaks are possible” indica que no existen memory leaks, es decir, no quedó memoria sin liberar al terminar la ejecución.

2. ¿Por qué se usa sizeof(int) en lugar del valor literal 4? ¿Qué ventaja ofrece en portabilidad entre arquitecturas?   
   Se hace de esta forma porque aunque para algunas arquitecturas de procesador el valor del tamaño para un entero sea 4 para otras el valor será distinto, al usar sizeof en lugar de dejar un valor fijo nuestro código puede ser ejecutado en distintas arquitecturas sin presentar problemas, dándole portabilidad entre arquitecturas de procesadores.

3. ¿Que devuelve malloc Cuando no hay memoria disponible? ¿Por qué es crítico verificar ese valor antes de usarlo?  
   Cuando malloc falla este retorna el valor de NULL, es importante verificar que el valor que retorna sea distinto a NULL porque de lo contrario podría suceder que se intente acceder a una dirección inválida causando un error de fragmentación lo que haría colapsar al programa. 

## 2.3 Actividad: Código con bugs de memoria

Comando ejecutado 

```
gcc -Wall -g -o buggy_mem buggy_mem.c
```

El comando no produjo una salida

Comando ejecutado

```
valgrind --leak-check=full --track-origins=yes ./buggy_mem
```

Salida del comando

```
==9799== Memcheck, a memory error detector
==9799== Copyright (C) 2002-2024, and GNU GPL'd, by Julian Seward et al.
==9799== Using Valgrind-3.25.1 and LibVEX; rerun with -h for copyright info
==9799== Command: ./buggy_mem
==9799== 
==9799== Invalid write of size 4
==9799==    at 0x400119F: main (buggy_mem.c:10)
==9799==  Address 0x4a5d054 is 0 bytes after a block of size 20 alloc'd
==9799==    at 0x48487C4: malloc (vg_replace_malloc.c:446)
==9799==    by 0x400117A: main (buggy_mem.c:8)
==9799== 
hola mundo
==9799== Invalid read of size 4
==9799==    at 0x40011ED: main (buggy_mem.c:19)
==9799==  Address 0x4a5d040 is 0 bytes inside a block of size 20 free'd
==9799==    at 0x484B78B: free (vg_replace_malloc.c:989)
==9799==    by 0x40011E8: main (buggy_mem.c:18)
==9799==  Block was alloc'd at
==9799==    at 0x48487C4: malloc (vg_replace_malloc.c:446)
==9799==    by 0x400117A: main (buggy_mem.c:8)
==9799== 
p[0] = 0
==9799== 
==9799== HEAP SUMMARY:
==9799==     in use at exit: 100 bytes in 1 blocks
==9799==   total heap usage: 3 allocs, 2 frees, 1,144 bytes allocated
==9799== 
==9799== 100 bytes in 1 blocks are definitely lost in loss record 1 of 1
==9799==    at 0x48487C4: malloc (vg_replace_malloc.c:446)
==9799==    by 0x40011B4: main (buggy_mem.c:13)
==9799== 
==9799== LEAK SUMMARY:
==9799==    definitely lost: 100 bytes in 1 blocks
==9799==    indirectly lost: 0 bytes in 0 blocks
==9799==      possibly lost: 0 bytes in 0 blocks
==9799==    still reachable: 0 bytes in 0 blocks
==9799==         suppressed: 0 bytes in 0 blocks
==9799== 
==9799== For lists of detected and suppressed errors, rerun with: -s
==9799== ERROR SUMMARY: 3 errors from 3 contexts (suppressed: 0 from 0)

```

## 2.4 Actividad: Identificar y corregir errores de memoria

1. Transcribe los mensajes que arroja Valgrind. ¿Cual mensaje corresponde a cada uno de los tres errores clásicos?  
     
   Valgrind detectó tres errores diferentes durante la ejecución del programa buggy\_mem.  
   **El primer error corresponde a un buffer overflow:** 

 

```
Invalid write of size 4
at 0x400119F: main (buggy_mem.c:10)
Address 0x4a5d054 is 0 bytes after a block of size 20 alloc'd
```

	Este error ocurre porque el ciclo utiliza la condición i \<= 5 en lugar de i \< 5, escribiendo fuera de  
	los límites del arreglo reservado dinámicamente.

	**El segundo error corresponde a un user-after-free :** 

```
Invalid read of size 4
at 0x40011ED: main (buggy_mem.c:19)
Address 0x4a5d040 is 0 bytes inside a block of size 20 free'd
```

Este error sucede porque el programa intenta acceder a p\[0\] después de haber liberado la  
memoria con free(p). 

**El tercer error corresponde a un memory leak:**	

```
100 bytes in 1 blocks are definitely lost in loss record 1 of 1
```

	  
	Esto ocurre porque la memoria reservada para el puntero que nunca fue liberada mediante free(q).

	**El resumen final de Valgrind fue:**   
	

```
ERROR SUMMARY: 3 errors from 3 contexts
```

Confirmando la presencia de los tres errores.	

2. Corrija el programa (buggy mem fixed.c) y verifique con Valgrind que no queda ningún error ni fuga.

	  
Código corregido   
	Archivo fixed\_buggy\_mem.c

```c
// buggy_mem.c -- NO ejecutar sin Valgrind
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
  /* ERROR 1: buffer overflow */
  int *p = malloc(5 * sizeof(int));
  for (int i = 0; i < 5; i++) /* <= en vez de < */
    p[i] = i;

  /* ERROR 2: memory leak (nunca se llama free(q)) */
  char *q = malloc(100);
  strcpy(q, "hola mundo");
  printf("%s\n", q);
  free(q);

  /* ERROR 3: use-after-free */
  printf("p[0] = %d\n", p[0]); /* acceso ilegal */
  free(p);

  return 0;
}
```

	Comprobación con valgrind  
	

```
valgrind --leak-check=full --track-origins=yes ./fixed_buggy_mem
```

	  
	Salida obtenido 

```
==17167== Memcheck, a memory error detector
==17167== Copyright (C) 2002-2024, and GNU GPL'd, by Julian Seward et al.
==17167== Using Valgrind-3.26.0 and LibVEX; rerun with -h for copyright info
==17167== Command: ./fixed_buggy_mem
==17167==
hola mundo
p[0] = 0
==17167==
==17167== HEAP SUMMARY:
==17167==     in use at exit: 0 bytes in 0 blocks
==17167==   total heap usage: 2 allocs, 2 frees, 1,124 bytes allocated
==17167==
==17167== All heap blocks were freed -- no leaks are possible
==17167==
==17167== For lists of detected and suppressed errors, rerun with: -s
==17167== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

3. ¿Que consecuencias puede tener un use-after-free en un programa real en términos de seguridad y estabilidad del sistema?  
     
   Un user-after-free puede provocar fallos graves de estabilidad y seguridad. En términos de estabilidad, puede causar comportamientos impredecibles, corrupción de memoria, resultados incorrectos o cierres inesperados del programa. En términos de seguridad, este tipo de vulnerabilidad puede ser explotada por atacantes para ejecutar código arbitrario, modificar datos sensibles o  tomar control parcial del sistema. Debido a esto, los errores use-after-free son considerados vulnerabilidades críticas en software.

# Traducción de direcciones — Base & Bounds

## 3.1 Actividad: Simulador

Aqui solo era agregar el archivo compartido en la guía 

## 3.2 Actividad: Base & Bounds — Análisis

1. Compile y ejecute. Muestre la salida completa. ¿Qué ocurre al acceder a VA=64 y VA=100 en el Proceso A? ¿Que haría el SO real ante esta excepción?

	Salida obtenida al ejecutar el programa 

```
./base_bounds
--- Proceso A (base=32, bounds=64) ---
 VA=  0 -> PA= 32
 VA= 10 -> PA= 42
 VA= 63 -> PA= 95
 [EXCEPCION] VA=64 viola bounds=64
 [EXCEPCION] VA=100 viola bounds=64
--- Proceso B (base=128, bounds=80) ---
 VA=  0 -> PA=128
 VA= 10 -> PA=138
 VA= 63 -> PA=191
 VA= 64 -> PA=192
 [EXCEPCION] VA=100 viola bounds=80
```

Al acceder a la VA 64 se comprueba que no sea mayor que el bounds y como no lo es se realiza la traducción de esta a una dirección física obteniendo la PA 192\. En el caso de la VA 100 esta es mayor que el bounds por lo que se incumple la condición y se arroja un fallo de protección.

2. Agregue un Proceso C (base=0, bounds=32) al programa y traduzca las mismas VAs.¿Puede el Proceso A acceder a las direcciones del Proceso C directamente? Justifique.

	Programa modificado

```c
// base_bounds.c
#include <stdio.h>

typedef struct {
  int base;
  int bounds;
} Registro;

/* Traduce VA -> PA; imprime excepcion si viola bounds */
int traducir(Registro r, int va) {
  if (va < 0 || va >= r.bounds) {
    printf(" [EXCEPCION] VA=%d viola bounds=%d\n", va, r.bounds);
    return -1;
  }
  return r.base + va;
}

int main() {
  Registro procA = {32, 64};  /* base=32, bounds=64 */
  Registro procB = {128, 80}; /* base=128, bounds=80 */
  Registro procC = {0, 32}; /* base=0, bounds=32 */
  int vas[] = {0, 10, 63, 64, 100};
  int n = sizeof(vas) / sizeof(vas[0]);

  printf("--- Proceso A (base=%d, bounds=%d) ---\n", procA.base, procA.bounds);
  for (int i = 0; i < n; i++) {
    int pa = traducir(procA, vas[i]);
    if (pa != -1)
      printf(" VA=%3d -> PA=%3d\n", vas[i], pa);
  }
  printf("--- Proceso B (base=%d, bounds=%d) ---\n", procB.base, procB.bounds);
  for (int i = 0; i < n; i++) {
    int pa = traducir(procB, vas[i]);
    if (pa != -1)
      printf(" VA=%3d -> PA=%3d\n", vas[i], pa);
  }
  printf("--- Proceso C (base=%d, bounds=%d) ---\n", procC.base, procC.bounds);
  for (int i = 0; i < n; i++) {
    int pa = traducir(procC, vas[i]);
    if (pa != -1)
      printf(" VA=%3d -> PA=%3d\n", vas[i], pa);
  }
  return 0;
}
```

	Salida tras ejecución del programa

```
 ./base_bounds
--- Proceso A (base=32, bounds=64) ---
 VA=  0 -> PA= 32
 VA= 10 -> PA= 42
 VA= 63 -> PA= 95
 [EXCEPCION] VA=64 viola bounds=64
 [EXCEPCION] VA=100 viola bounds=64
--- Proceso B (base=128, bounds=80) ---
 VA=  0 -> PA=128
 VA= 10 -> PA=138
 VA= 63 -> PA=191
 VA= 64 -> PA=192
 [EXCEPCION] VA=100 viola bounds=80
--- Proceso C (base=0, bounds=32) ---
 VA=  0 -> PA=  0
 VA= 10 -> PA= 10
 [EXCEPCION] VA=63 viola bounds=32
 [EXCEPCION] VA=64 viola bounds=32
 [EXCEPCION] VA=100 viola bounds=32
```

El proceso A no puede acceder a direcciones del proceso C porque sus espacios de direcciones son distintos, estos están definidos por dos límites el inferior que es el base le cual es inclusivo y el superior que es el bounds que no es inclusivo, tenido esto en cuenta para el proceso C tenemos base de 0 y bounds de 32 y para el proceso A tenemos base de 32 y bounds de 64 por lo que en sus espacios de direcciones no comparten ninguna dirección. 

3. ¿Cual es la limitación principal del esquema base & bounds que motiva el surgimiento de la segmentación?

   Su mayor limitación es algo conocido como fragmentación interna, esta se refiere a los huecos que quedan entre los elementos del mapa de memoria y que no pueden ser aprovechados para crear más procesos.

# Segmentación

## 4.1 Traducción manual con tabla de segmentos

**Dado un espacio de 14 bits (2 bits de selector \+ 12 bits de offset), use la siguiente tabla de**  
**segmentos:**

**Tabla de segmentos:**

| Segmento | Base (PA) | Tamaño | Crecimiento | Selector |
| ----- | ----- | ----- | ----- | ----- |
| Code | 0x4000 | 2 KB | positivo | 00 |
| Heap | 0x6000 | 3 KB | positivo | 01 |
| Stack | 0x2800 | 2 KB | negativo | 11 |

**2 KB \= 0x800 bytes**  
**3 KB \= 0xC00 bytes**

**Traduzca manualmente y complete la tabla:**

| VA (hex) | Selector | Offset | Segmento | PA o Excepción |
| :---: | :---: | :---: | :---: | :---: |
| 0x03A0 | 00 | 0x3A0 | Code | 0x43A0 |
| 0x1800 | 01 | 0x800 | Heap | 0x6800 |
| 0x3C00 | 11 | 0xC00 | Stack | Excepción de segmentación |
| 0x0C00 | 00 | 0xC00 | Code | Excepción de segmentación |
| 0x2200 | 10 | — | ??? | Excepción por segmento inválido |

1. **Muestre el cálculo paso a paso para cada VA.**  
     
   **A- Traducción:**  
     
   **VA:** 0x03A0  
   **Selector:** 00  
   **offset:** 0x03A0  
   **Validación:**   
   0x03A0 \< 0x0800  
     
   **Cálculo:**   
   **PA \=** 0x4000 \+ 0x03A0  
   **PA \=** 0x43A0  
     
     
   **B- Traducción:**   
     
   **VA:** 0x1800  
   **Selector:** 01  
   **offset:** 0x0800  
   **Validación:**   
   0x0800 \< 0x0C00  
     
   **Cálculo:**   
   **PA \=** 0x6000 \+ 0x0800  
   **PA \=** 0x6800  
     
   **C- Traducción:**   
     
   **VA:** 0x3C00  
   **Selector:** 11  
   **offset:** 0x0C00  
   **Validación:**   
   El stack tiene tamaño 0x0800  
   **Como:**  
   0x0C00 \> 0x0800  
   En este caso el offset que sería 0x0C00 excede el tamaño permitido.  
   **Resultado:**  
   Excepción de segmentación  
     
   **D- Traducción:**   
     
   **VA:** 0x0C00  
   **Selector:** 00  
   **offset:** 0x0C00  
   **Validación:**   
   El segmento Code tiene tamaño 0x0800  
   **Como:**  
   0x0C00 \> 0x0800  
   En este caso el offset que sería 0x0C00 excede el tamaño permitido.  
   **Resultado:**  
   Excepción de segmentación  
     
   **E- Traducción:**  
     
   **VA:** 0x2200  
   **Selector:** 10  
   **Validación:**  
   No existe ningún segmento asociado al selector 10 en la tabla.  
   **Resultado:**  
   Excepción por segmento inválido  
     
2. **¿Por qué el Stack crece en dirección negativa? ¿Qué ajuste especial requiere la fórmula al calcular el PA?**  
     
   El stack crece en dirección negativa porque, en la mayoría de arquitecturas, las nuevas llamadas a funciones y variables locales se almacenan hacia direcciones de memoria menores. Debido a esto, el cálculo de la dirección física no se realiza como Base \+ Offset, sino restando desde el límite superior del segmento.  
     
   **La fórmula utilizada es:**

PA \= Base \- (Tamaño \- Offset)

Esto permite que el stack pueda ir hacia abajo en memoria.

3. **¿Qué ventaja tiene la segmentación frente a base & bounds en cuanto a utilización de la memoria física?**

	  
La segmentación permite dividir el espacio de direcciones en múltiples regiones independientes como code, heap y stack, cada una con tamaños y ubicaciones diferentes en memoria física. Esto mejora el aprovechamiento de memoria física porque no es necesario reservar un único bloque contiguo grande para todo el proceso. Cada segmento puede ubicarse en diferentes posiciones de memoria según la disponibilidad.

En cambio, el esquema simple de base & bounds requiere una única región contigua para todo el espacio del proceso.  
	

4. **¿Qué es la fragmentación externa? ¿Por qué surge con segmentación? Ilustre con un diagrama de bloques de memoria.**  
     
   La fragmentación externa ocurre cuando la memoria libre queda dividida en pequeños espacios dispersos entre segmentos ocupados. Aunque la suma de memoria libre pueda ser suficiente, puede no existir un bloque contiguo lo bastante grande para alojar un nuevo segmento.  
     
   Esto sucede en segmentación porque los segmentos tienen tamaños variables y se cargan o liberan dinámicamente.  
     
   **Ejemplo:**  
     
   ![][image2]  
     
   Tras liberar B y D, quedan tres huecos: 65 KB, 60 KB y 20 KB al final, sumando 145 KB libres en total. Aun así, un nuevo proceso que necesite 100 KB contiguos no puede ser atendido, ya que ningún hueco individual alcanza ese tamaño la memoria está fragmentada en espacios discontinuos separados por los segmentos ocupados A, C y E.

# Paginación

## 5.1 Actividad: Cálculo de la tabla de páginas

**Considere el sistema:**

| Parámetro | Valor |
| :---- | :---- |
| Espacio virtual | 32 bits |
| Tamaño de página | 4 KB \= 2¹² bytes |
| Espacio físico | 20 bits |
| Tamaño de PTE | 4 bytes |

1. **¿Cuántos bits se necesitan para el VPN y cuantos para el offset? Muestre el cálculo.**  
     
   El tamaño de la página es: 4 KB \= 4096 bytes \= 2¹² bytes  
   num pages \= 232212 \= 220  
   2n(VPN) \= num pages \= 220  
   n(VPN)  \= 20  
   2n(offset) \= size(page)  
   2n(offset) \= 212  
   n(offset) \= 12  
   la cantidad de bits que se necesitan para el VPN debe ser la suficiente para poder representar los num pages, en este caso se necesitan 20 bits   
     
2. **¿Cuántas entradas tiene la tabla de páginas de un proceso?**  
     
   Cada combinación posible del VPN necesita una entrada en la tabla de páginas.  
   **Como el VPN tiene 20 bits:**  
   Entradas=2²⁰  
   Entradas=1,048,576  
     
   **La tabla de páginas tiene 1,048,576 entradas.**   
     
3. **¿Cuánta memoria ocupa la tabla de páginas completa? ¿Es razonable ese tamaño para cada proceso?**  
   **size(Page table) \= size(PTE)\*num pages**  
   **size(Page table) \= 4 \* 2²⁰ \= 222**  
   **size(Page table) \= 4,194,304 bytes \= 4 MB**  
     
   **Resultado:**  
   La tabla de páginas completa ocupa aproximadamente 4 MB por proceso.

	  
Respecto a si es razonable al analizar los resultados vemos que tenemos un espacio físico de	 1MB mientras que necesitamos destinar 4 MB para almacenar la page table, osea que se necesitan 4 veces el espacio físico para guardar la page table este tamaño es bastante grande por lo que no sería razonable.

4. **¿Cuántos bits necesita el PFN dentro de la PTE? ¿Qué información almacena los bits restantes? Mencione al menos 3 bits de control y su función.**

	*El espacio físico: 2²⁰ bytes \= 1 MB*  
*Cada página ocupa 4 KB*  
*offset es de 12 bits*

	**num frames \= size(PM)size(Frame)**  
	**num frames \= 220212 \= 28**  
**2n(PFN) \= num frames**  
2n(PFN)=28  
n(PFN) \= 8

Los bits restantes de la PTE se utilizan como bits de control.

**Bits de control:**   
 

* Los bits más significativos que acompañan al PFN en la PTE, se dividen en lo siguiente:  
* Bit de validez(V): indica si la traducción es válida osea si un PTE puede o no ser usado   
* Bit de referencia(R): Indica si una página ha sido accedida  
* Bit de presencia(P): Indica si una página está en memoria física o si está en el disco duro   
* Bit sucio(M): Indica si la página ha sido modificada  
* Bits de protección(Prot): definen las características del frame respecto a los permisos que se tienen sobre este pedazo de memoria (Read-Write-Execute)

## 5.2 Actividad: Simulador de paginación

En este punto solo debíamos guardar el archivo paging\_sim.c

## 5.3 Actividad: Simulador — Análisis

1. Compile y ejecute el simulador. Muestre la salida completa.

```
./paging_sim
VA                     VPN    Offset   PFN    PA
-----------------------------------------------------
VA=0x00 VPN= 0 Offset= 0 -> PFN= 3 PA=0x30
VA=0x0F VPN= 0 Offset=15 -> PFN= 3 PA=0x3F
VA=0x20 VPN= 2 Offset= 0 -> PFN= 7 PA=0x70
VA=0x35 VPN= 3 Offset= 5 -> PFN= 2 PA=0x25
VA=0x10 VPN= 1 Offset= 0 -> PAGE FAULT (pagina no presente)
VA=0xA3 VPN=10 Offset= 3 -> PFN= 4 PA=0x43
VA=0xC8 VPN=12 Offset= 8 -> PFN= 6 PA=0x68
VA=0xF0 VPN=15 Offset= 0 -> PAGE FAULT (pagina no presente)
```

2. ¿Qué ocurre con las VAs 0x10 y 0xA3? ¿Qué debería hacer el SO real ante un page fault?

   Con la VA 0xA3, no produce un error y se puede traducir correctamente, respecto a la VA 0x10 se produce un page fault, esto significa que el bit de presencia para esta página está en 0, lo que se debe hacer en un caso real es verificar si la referencia es válida, luego se localiza la página en el disco duro, se confirma que sea válida, en caso de que sea válida se debe traer la página del disco para la memoria física a un frame, en caso de que no se tenga espacio disponible se usa una política de reemplazo para reemplazar uno de los frames.

   

3. ¿Cuántos accesos a memoria física requiere completar una instrucción load con tabla de páginas de un solo nivel? ¿Por qué es costoso y qué solución de hardware existe?  
     
   **Con una tabla de páginas de un solo nivel se requieren dos accesos a memoria física:**  
     
1. Un acceso para leer la Page Table Entry.  
2. Otro acceso para leer el dato real en memoria física.  
     
   Esto incrementa el tiempo de acceso porque cada referencia de memoria necesita realizar una operación de acceso que es costosa. La solución de hardware utilizada es el Translation Lookaside Buffer (TLB), una memoria caché utilizada para almacenar traducciones recientes.  
     
   Cuando ocurre un TLB hit, la traducción se obtiene directamente desde la TLB y solo se requiere un acceso a memoria física para obtener el dato.  
     
4. ¿Que ventaja tiene la paginación sobre la segmentación en cuanto al fenómeno de fragmentación?  
     
   La principal ventaja de la paginación es que elimina la fragmentación externa. Como todas las páginas y frames tienen tamaño fijo, cualquier frame libre puede utilizarse para almacenar cualquier página, evitando huecos dispersos de tamaños variables en memoria física. En cambio, la segmentación utiliza segmentos de tamaño variable, lo que produce fragmentación externa con el tiempo.

# Gestión de espacio libre

## 6.1 Actividad: Simulación de estrategias de asignación

**Estado inicial de la lista libre:**

| Dirección inicio | Tamaño (bytes) |
| :---: | :---: |
| 0x0100 | 100 |
| 0x0200 | 500 |
| 0x0400 | 200 |
| 0x0500 | 300 |
| 0x0700 | 600 |

Solicitudes en orden: malloc(212) · malloc(417) · malloc(98) · malloc(426)

1. **Para cada solicitud indique que bloque asigna first fit. Muestre la lista libre resultante tras las 4 asignaciones.**  
     
   First Fit selecciona el primer bloque libre que sea suficientemente grande para satisfacer cada solicitud.  
     
   **Solicitud 1:** malloc(212)  
     
   **El primer bloque capaz de alojar 212 bytes es:**

   0x0200 \=  500 bytes  
     
   **Se asignan 212 bytes y quedan:**  
     
   500 \- 212 \= 288 bytes libres  
     
   **Nueva lista:**  
   

| Dirección | Tamaño |
| :---: | :---: |
| 0x0100 | 100 |
| 0x02D4 | 288 |
| 0x0400 | 200 |
| 0x0500 | 300 |
| 0x0700 | 600 |

   

   **Solicitud 2:** malloc(417)

   

   Los bloques de 100, 288, 200 y 300 bytes no alcanzan.

   

   **Se utiliza:**

   

   0x0700 \= 600 bytes

   

   **Quedan:**

   

   600 \- 417 \= 183 bytes

   

   **Nueva lista:**

   

| Dirección | Tamaño |
| :---: | :---: |
| 0x0100 | 100 |
| 0x02D4 | 288 |
| 0x0400 | 200 |
| 0x0500 | 300 |
| 0x08A1 | 183 |

   

   **Solicitud 3:** malloc(98)

   

   **El primer bloque suficiente es:**

   

   0x0100 \= 100 bytes

   

   **Quedan:**

   

   100 \- 98 \= 2 bytes

   

   **Nueva lista:**

   

| Dirección | Tamaño |
| :---: | :---: |
| 0x0162 | 2 |
| 0x02D4 | 288 |
| 0x0400 | 200 |
| 0x0500 | 300 |
| 0x08A1 | 183 |

   

   **Solicitud 4:** malloc(426)

   

   Ningún bloque disponible tiene tamaño suficiente.

   

   **Resultado:**

   

   La asignación falla.

   

   **Lista final con First Fit:**

   

| Dirección | Tamaño |
| :---: | :---: |
| 0x0162 | 2 |
| 0x02D4 | 288 |
| 0x0400 | 200 |
| 0x0500 | 300 |
| 0x08A1 | 183 |

   

   

2. **Repita con best fit. ¿Cambia el resultado?**

   El best fit primero mira en qué bloques se puede alojar lo que se está solicitando y escoge de estos el que tenga menor tamaño 

   

   **Solicitud 1:** malloc(212)

   Los bloques que tienen suficiente espacio para reservar son:  
   \- 0x0200 con un tamaño de 500 bytes

   \- 0x0500 con un tamaño de 300 bytes

   \- 0x0700 con un tamaño de 600 bytes

   Elegimos el bloque 0x0500 porque es el que tiene el menor tamaño, asignamos los 212 bytes y nos quedarían 88 bytes libres para ese bloque, la nueva lista quedaría de la siguiente manera

| Dirección inicio | Tamaño (bytes) |
| :---: | :---: |
| 0x0100 | 100 |
| 0x0200 | 500 |
| 0x0400 | 200 |
| 0x05d4 | 88 |
| 0x0700 | 600 |

	**Solicitud 2:** malloc(417):  
	Los bloques que tienen suficiente espacio para reservar son:  
\- 0x0200 con un tamaño de 500 bytes  
\- 0x0700 con un tamaño de 600 bytes  
Elegimos el bloque 0x0200 porque es el que tiene el menor tamaño, asignamos los 417 bytes y nos quedan 83 bytes libres para este bloque y la nueva lista sería la siguiente

| Dirección inicio | Tamaño (bytes) |
| :---: | :---: |
| 0x0100 | 100 |
| 0x0200 | 83 |
| 0x0400 | 200 |
| 0x05d4 | 78 |
| 0x0700 | 600 |

	**Solicitud 3:** malloc(98)  
Los bloques que tienen suficiente espacio para reservar son:  
\- 0x0100 con un tamaño de 100 bytes  
\- 0x0400 con un tamaño de 200 bytes  
\- 0x0700 con un tamaño de 600 bytes  
Se elige el bloque 0x0100 porque es el que tiene el menor tamaño, asignamos los 98 bytes y nos quedarían 2 bytes libres en el bloque y la nueva lista sería la siguiente   
	  
	

| Dirección inicio | Tamaño (bytes) |
| :---: | :---: |
| 0x0162 | 2 |
| 0x0200 | 83 |
| 0x0400 | 200 |
| 0x05d4 | 78 |
| 0x0700 | 600 |

	**Solicitud 4:** malloc(426)  
Los bloques que tienen suficiente espacio para reservar son:  
\- 0x0700 con un tamaño de 600 bytes  
Se elige el bloque 0x0700 porque es el único con el tamaño necesario donde  asignamos los 426  bytes y nos quedarían 174 bytes libres en el bloque y la nueva lista sería la siguiente 

| Dirección inicio | Tamaño (bytes) |
| :---: | :---: |
| 0x0162 | 2 |
| 0x0200 | 83 |
| 0x0400 | 200 |
| 0x05d4 | 78 |
| 0x08aa | 174 |

3. **¿Cúal estrategia genera más fragmentación externa en este caso? ¿Cúal la minimiza?**  
   En este caso, First Fit genera más fragmentación externa porque deja bloques medianos dispersos y desperdicia parte del bloque de 500 bytes desde la primera asignación.  
     
   Best Fit minimiza mejor la fragmentación externa porque intenta usar el bloque más ajustado posible para cada solicitud, preservando bloques grandes para futuras asignaciones. Mientras que Best Fit también puede producir muchos bloques pequeños difíciles de reutilizar.  
     
4. **¿Qué es el coalescing? Ilustre un caso donde su ausencia provoca que una solicitud de 250 bytes falle aunque haya suficiente memoria total libre.**  
     
   El coalescing consiste en que al momento de liberar un bloque de memoria, se debe de revisar si los bloques cercanos respecto a las direcciones  también son memoria libre y en el caso de que lo sean se deben unir  los bloques para que de esta forma el bloque de memoria libre sea lo más grande posible  
     
   **Ejemplo**  
     
   Inicialmente tenemos los siguiente bloques de memoria  
   

| Bloque  | Tamaño |
| :---- | :---- |
| Libre | 100 bytes |
| Ocupado | 200 bytes |
| Libre | 150 bytes |
| Ocupado | 120 bytes |

   Luego realizamos la liberación del cuarto bloque, en un sistema sin coalescing obtendremos lo siguiente 

   

| Bloque  | Tamaño |
| :---- | :---- |
| Libre | 100 bytes |
| Ocupado | 200 bytes |
| Libre | 150 bytes |
| Libre | 120 bytes |

   

   La memoria libre total: 

   100 \+ 150 \+ 120 \= 370 bytes

   

   Sin embargo, si los bloques de 150 y 120 bytes no se fusionan, una solicitud de: 

   

   malloc(250)

   

   Fallará porque no existe un bloque contiguo suficientemente grande. En cambio con coalescing los bloques libres contiguos podrían combinarse para satisfacer la solicitud.

   

5. **¿Qué es la fragmentación interna? ¿Cuando aparece típicamente al usar un slab allocator?**  
     
   La fragmentación interna ocurre cuando un proceso utiliza solamente una parte del bloque asignado y el espacio restante queda desperdiciado dentro del mismo bloque.  
     
   En un slab allocator esto aparece típicamente porque los objetos se almacenan en bloques de tamaño fijo. Por ejemplo, si el allocator maneja bloques de 64 bytes y un objeto necesita únicamente 50 bytes, los 14 bytes restantes quedan inutilizados, produciendo fragmentación interna.

## 6.2 Actividad: Fragmentación

En este punto solo debemos guardar el archivo fragmentation.c

Se compila con: 

```
gcc -Wall -o fragmentation fragmentation.c
```

Salida al compilar

Ejecución

```
./fragmentation 
malloc(  16) -> 0x55c65cf602a0
malloc(  32) -> 0x55c65cf606d0
malloc(  64) -> 0x55c65cf60700
malloc( 128) -> 0x55c65cf60750
malloc( 256) -> 0x55c65cf607e0
malloc( 512) -> 0x55c65cf608f0
malloc(1024) -> 0x55c65cf60b00
malloc( 512) -> 0x55c65cf60f10
malloc( 256) -> 0x55c65cf61120
malloc( 128) -> 0x55c65cf61230

Liberando bloques en indices pares...

malloc(1500) -> 0x55c65cf612c0 [exito]
```

## 6.3 Actividad: Fragmentación en glibc — Análisis

1. ¿Son consecutivas en memoria las direcciones asignadas? ¿Qué patrón de separación observa entre bloques contiguos?

	La diferencia entre la dirección 2 y la dirección 1 es de 1072  
	La diferencia entre la dirección 3 y la dirección 2 es de 48  
	La diferencia entre la dirección 4 y la dirección 3 es de 80  
	La diferencia entre la dirección 5 y la dirección 4 es de 144  
	La diferencia entre la dirección 6 y la dirección 5 es de 272  
	La diferencia entre la dirección 7 y la dirección 6 es de 528  
	La diferencia entre la dirección 8 y la dirección 7 es de 1040  
	La diferencia entre la dirección 9 y la dirección 8 es de 528  
	La diferencia entre la dirección 10 y la dirección 9 es de 272  
	  
Las direcciones asignadas usando malloc parecen cercanas pero en realidad no son consecutivas entre sí como se puede observar en el cálculo anterior de la diferencia entre las direcciones. Respecto al patrón de separación podemos ver que la diferencia entre las direcciones es para todos los casos excepto el primero igual al tamaño reservado sumado junto a 16 bytes, los cuales suelen representar la metadata del bloque o una alineación a 16 bytes.  
	

2. ¿Tiene éxito la asignación final de 1500 bytes? Explique el resultado en términos de fragmentación.  
     
   Sí, la asignación final fue exitosa:  
 


```
malloc(1500) -> 0x55c65cf612c0 [exito]
```

   

   Aunque se liberaron varios bloques intermedios, glibc logró encontrar suficiente espacio libre para satisfacer la solicitud de 1500 bytes.

   

   Esto indica que:

   

1. el heap todavía disponía de memoria libre contigua suficiente  
2. el allocator pudo extender el heap o reutilizar regiones disponibles  
3. la fragmentación externa no era todavía lo suficientemente severa para impedir la asignación  
     
   En este caso sí existe fragmentación, porque quedaron huecos tras liberar los índices pares, pero no fue suficiente para provocar un fallo de asignación.  
     
3. Consulta: ¿Cuál es la diferencia entre el allocator de usuario (malloc/glibc) y el del kernel (buddy system, slab)? ¿Por qué existen dos niveles de gestión de memoria?  
     
   El allocator de usuario, como malloc() en glibc, administra el heap de cada proceso y trabaja sobre memoria virtual entregada por el sistema operativo.  
     
   **Su función es:**  
     
1. dividir bloques  
2. reutilizar memoria liberada  
3. manejar fragmentación  
4. entregar memoria dinámica a programas de usuario  
     
   En cambio, el allocator del kernel administra directamente la memoria física del sistema operativo.  
     
   **El kernel utiliza mecanismos como:**  
     
* Buddy System: divide memoria en bloques de tamaños potencia de dos para asignaciones grandes y eficientes.  
* Slab Allocator: mantiene cachés de objetos pequeños reutilizables para reducir overhead y fragmentación.  
    
  **Existen dos niveles de gestión porque las necesidades son diferentes:**  
    
* Los programas de usuario necesitan una interfaz flexible y sencilla como malloc().  
* El kernel necesita control directo, eficiencia y administración global de toda la RAM física.  
    
  De esta manera, el sistema operativo administra memoria física a bajo nivel, mientras que cada proceso administra su propio heap a nivel de usuario.

# TLBs — Translation Lookaside Buffer

Compilación con la opción de optimización

```
gcc -O -o tlb_locality tlb_locality.c
```

## 7.1 Actividad: Localidad y TLB — Análisis

**1\. ¿Cuántas veces más lento es el acceso aleatorio frente al secuencial? Muestre el promedio de 3 ejecuciones de tlb locality.**

Primera salida

```
./tlb_locality 
Secuencial :     3.17 ms (sum=8796090925056)
Aleatorio :    44.89 ms (sum=8796090925056)
```

Segunda salida

```
./tlb_locality 
Secuencial :     4.98 ms (sum=8796090925056)
Aleatorio :    46.94 ms (sum=8796090925056)
```

Tercera salida 

```
./tlb_locality 
Secuencial :     4.98 ms (sum=8796090925056)
Aleatorio :    46.94 ms (sum=8796090925056)
```

Promedio para secuencial: 4.38  
Promedio para aleatorio: 46.26

Al utilizar el acceso secuencial somos 10.56 más rápidos que usando el acceso aleatorio 

**2\. Explique con el modelo del TLB por qué el acceso aleatorio es más lento. ¿Qué ocurre con el hit rate en cada caso?**

El acceso secuencial presenta alta localidad espacial porque los datos consecutivos suelen encontrarse dentro de las mismas páginas o en páginas cercanas. Esto permite que las traducciones de VPN a PFN permanezcan en el TLB, ocurran muchos TLB hits y se reduzcan los accesos a la tabla de páginas. Por esta razón, el acceso secuencial es mucho más rápido.

Por otro lado, el acceso aleatorio rompe la localidad espacial y temporal, ya que el programa accede constantemente a páginas diferentes. Esto provoca más TLB misses, aumentando las consultas a la tabla de páginas causando un mayor tiempo de traducción de direcciones. Por lo tanto, el acceso secuencial produce un alto hit rate, mientras que el acceso aleatorio genera un bajo hit rate.

**3\. Si el tamaño de página fuera 64 KB en lugar de 4 KB, ¿mejoraría o empeoraría la situación con accesos aleatorios? Justifique desde el punto de vista del TLB y del uso de memoria.**

Con páginas de 64 KB, cada entrada del TLB cubriría una región mucho más grande de memoria. Esto mejoraría el rendimiento de los accesos aleatorios porque serían necesarias menos páginas para cubrir el arreglo completo, el TLB podría almacenar traducciones útiles para regiones más amplias y disminuiría la cantidad de TLB misses.

Sin embargo, también existen desventajas. El uso de páginas más grandes incrementa la fragmentación interna, provoca un mayor desperdicio de memoria dentro de cada página. En general, para accesos aleatorios el TLB podría beneficiarse de páginas más grandes, aunque con un mayor costo en el uso de memoria.

## 7.2 Actividad: Comportamiento de los TLB

**Responda conceptualmente estas preguntas:**

**1\. Un TLB con 64 entradas (fully associative) y páginas de 4 KB. ¿Cuánta memoria puede cubrir sin generar misses? ¿Es suficiente para un proceso moderno típico?**

Para una TLB con esas características podríamos guardar la traducción para 64 páginas, teniendo en cuenta que cada página tiene un tamaño de 4 KB, para obtener la cantidad de memoria que puede cubrir sin generar misses debemos de multiplicar el tamaño de página por la cantidad de entradas   
Cantidad de memoria que puede cubrir \= 64\*4kb \= 256KB

Respecto a si es suficiente se considera que para procesos modernos típicos no sería suficiente porque estos suelen tener espacios de memoria del orden de varios MB o incluso de GB, entonces con una TLB tan pequeña se generarían muchos TLB misses de forma que no aportaría prácticamente al rendimiento.

**2\. Consulte: ¿Qué es un TLB shootdown y en qué situación ocurre en sistemas multiprocesador? ¿Por qué es una operación costosa?**

Un TLB shootdown ocurre cuando el sistema operativo necesita invalidar entradas del TLB en múltiples CPUs. Esto sucede normalmente cuando cambia una tabla de páginas, se libera memoria, se modifica un mapeo virtual o cambian los permisos de acceso de una página.

En sistemas multiprocesador, cada CPU puede tener almacenadas copias distintas de las traducciones de memoria en su propio TLB. Por eso, cuando el sistema operativo modifica una traducción, debe asegurarse de que todas las CPUs eliminen las entradas antiguas para evitar inconsistencias.

Para lograrlo, el sistema operativo envía interrupciones entre procesadores (IPIs) con el fin de obligar a cada núcleo a invalidar sus entradas del TLB. Esta operación es costosa porque requiere sincronización entre CPUs, interrumpe temporalmente la ejecución normal de los procesos, invalida traducciones útiles almacenadas en caché y además genera overhead de comunicación entre los distintos núcleos del procesador.

**3\. Explique la diferencia entre TLB gestionado por hardware (CISC/x86) y por software (RISC/MIPS). ¿Cuál ofrece mayor flexibilidad al diseñador del SO y por qué?**

En arquitecturas como x86, el TLB es manejado principalmente por hardware. Cuando ocurre un TLB miss, el hardware consulta automáticamente la tabla de páginas, carga la traducción correspondiente en el TLB y luego reanuda la ejecución del programa. Este mecanismo simplifica el diseño del sistema operativo y hace que la gestión de memoria sea más transparente para el programador. En arquitecturas como las MIPS clásicas, el TLB puede ser manejado por software. En este caso, cuando ocurre un miss, se genera una excepción y el sistema operativo toma el control para decidir cómo resolverla. Luego, el propio sistema operativo inserta manualmente la traducción adecuada dentro del TLB.

El manejo por software ofrece mayor flexibilidad al diseñador del sistema operativo, ya que permite implementar distintos formatos de tablas de páginas, facilita el uso de políticas personalizadas de reemplazo y brinda más control sobre la administración de memoria. Sin embargo, también aumenta la complejidad del sistema operativo y puede introducir más overhead durante la ejecución.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAlgAAAJYCAMAAACJuGjuAAADAFBMVEX////+/v79/v79/v39/f38/f38/fz7/fz7/Pz7/Pv6/Pv6+/v6+/r4+/r4+/n4+vn3+vn2+vj2+ff1+ff1+Pf1+Pb0+PXz+PXz9/Xy9/Xy9vTx9vTx9vPw9vPw9fPv9fLv9fHu9fHt9PDt8/Ds8+/r8u/r8u7q8u3p8e3p8ezo8Ozn8Ovm7+rl7+rl7unk7ujj7eji7efi7Ofh7Obg7Obg6+Xf6+Te6uTe6uPd6ePc6eLb6eHb6OHa6ODZ5+DY5t7W5t3W5d3V5NzU5NvT49rR4tnQ4tjP4dfO4NbM39XL39TK3tPJ3dPJ3dLI3NHG3NDF28/F2s/E2s7D2s7D2c3B2MzA2Mu/18q+1sm91sm81ci71ce61Ma508W408W30sS30sO10cK00MGz0MCyz7+xzr+xzr6vzb2uzbytzLysy7qqyrmpyrioybenyLenyLalx7WkxrSixbKhxLGfw7Cew6+dwq6cwa2awKyawKuZv6uYv6qXvqmWvaiVvaeUvaeTvKaSu6WRu6SRuqSQuqOPuaOOuaKNuKGMt6CLt5+Ktp+Jtp6ItZ2HtJyFs5qEspmCsZiBsZeBsJeAsJZ/r5V+rpR8rpN7rZJ6rJF5rJF4q5B2qo51qY1zqIxxp4pwpolvpYhtpIdspIZso4Zro4VpooRooYNooYJnoIJmoIFln4Bknn9jnn5inX1hnX1gnHxfm3tem3pdmnlcmnlbmXhamXdZmHZYl3VWlnRVlXNUlXJUlHJTlHFRk3BRk29Qkm9Pkm5OkW1MkGxLj2tKj2pKjmpJjmlIjWhHjGdFjGZEi2VDimRBiWJAiGI/iGE/h2A+h2A8hl46hV05hFw5hFs4g1s3g1o2glk1gVg0gVgzgFcyf1Yxf1UvflQvfVMufVIsfFEre1Apek8pek4neU0meEwkd0ojdkoidkkhdUgfdEYec0Ydc0UcckQacUIYcEEXb0AWbj8VbT4UbT0SbDwRazoPajkOaTkNaTgMaDYKZzUIZjQHZTIGZDIFZDEEYzCw2tSzAACAAElEQVR4XuydZUBbwba2JyFBgkNxd3enuFOKFUqFOhXq3tJCFahQd3f3FloKFHd3J7i7hYT4l0BCqdxz7r/vku7nB2TPzN5Jm5eZNWvWrAEAAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAuL/P7DfCyBmI/LJdOo3rDswg/aL96knZeoybgl2djOIP0D8XgDxCxygCzuBGRM23k3MhQUtsKuupf0pIu25fm8H8RuQsP4zbA3r+jCTmPk7Xbmq4d7UmHsZNGFJxUId/X8DEtZ/AdMwRPuZid6lb89NfHgWBqgAkH9vBPEHkLD+M7AhzNTvnhA2AR7KMF1WNKZ/QvwHIGH9R+BU4QU0c51KBnAyBcBg3CgkBztSSKL/94YQvwEJ6z9CLNPeSQVUHrXahNFJEpXiZz9GZIOTB7MIv7eE+BXICv2PwITY4QAQTJ9wpA7C8ThNy+49/QiA728n/t5yGknc8O9F/yhsvxdA/AIOMz4+ju1WNp3sxAwNmQqfftna2to5POXN+hOLUz3o38v+UeC/F0D8Bcznfmk+SQVV0drE/6GrmkJoj64R/TdMV+P3qn8OyMb6CwhxfhjADo7RJ3/s8uzjnaTsWrMxNEWe/1UlTFyJe7gSS/uTlJDmh+P7OqZaTeOktcPJNR7wr9java3m5+NmELbqLP4f+jqW418XFgz+h1OK197XUBAOsD31BXllqtvdkCMNTfUpBrIjhHkwSqC1mTTnxOvIcW93fUkOKgnTmpecPca40wKfT3VJ4z1j/cXB6i/CQoVowN4++b2URfnHhWWzhLswpnV2CdxxnTs/trFmkl/Gd2X3VxXn/rcjVnZ8RBR7qoAr11YEsfwFcN/Mh/Dm6yuu7gaShhs3pD/5jpu6F8YvnhegN8/gQJYFx+xnMlgyf+f8JTF0f+s/wL8tLLNrE5+cLA90/CxRXB8kWPcotbSNChPU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>

[image2]: <data:image/png;base64,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>
