---
title: Bucles
teaching: 40
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Escribir un bucle que aplique uno o más comandos por separado a cada archivo en un conjunto de archivos.
- Rastrear los valores que toma una variable de bucle durante la ejecución del bucle.
- Explicar la diferencia entre el nombre de una variable y su valor.
- Explicar por qué no se deben utilizar espacios y algunos caracteres de puntuación en los nombres de archivo.
- Demostrar cómo ver qué comandos se han ejecutado recientemente.
- Volver a ejecutar comandos ejecutados recientemente sin volver a escribirlos.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Cómo puedo realizar las mismas acciones en muchos archivos diferentes?

::::::::::::::::::::::::::::::::::::::::::::::::::

Los **bucles** son una construcción de programación que nos permite repetir un comando o conjunto de comandos
para cada elemento en una lista.
Como tal, son clave para mejorar la productividad a través de la automatización.
Al igual que los comodines y la autocompletación de pestañas, el uso de bucles también reduce la
cantidad de escritura requerida (y por lo tanto reduce la cantidad de errores de escritura).

Supongamos que tenemos varios archivos de datos de genoma llamados `basilisk.dat`, `minotaur.dat` y
`unicorn.dat`.
Para este ejemplo, usaremos el directorio `exercise-data/creatures` que solo tiene tres
archivos de ejemplo,
pero los principios se pueden aplicar a muchos más archivos a la vez.

La estructura de estos archivos es la misma: se presentan el nombre común, la clasificación y la fecha de actualización
en las primeras tres líneas, con secuencias de ADN en las líneas siguientes. Veamos los archivos:

```bash
$ head -n 5 basilisk.dat minotaur.dat unicorn.dat
```

Nos gustaría imprimir la clasificación de cada especie, que se encuentra en la segunda
línea de cada archivo.
Para cada archivo, tendríamos que ejecutar el comando `head -n 2` y enviarlo a `tail -n 1`.
Usaremos un bucle para resolver este problema, pero primero vamos a ver la forma general de un bucle,
usando el seudocódigo a continuación:

```bash
# La palabra "for" indica el comienzo de un comando "For-loop"
para cosa en lista_de_cosas `
# La palabra "do" indica el comienzo de una lista de ejecución de trabajos
hacer
    # La indentación dentro del bucle no es necesaria, pero ayuda a la legibilidad
    operación_usando / comando $cosa
# La palabra "done" indica el final de un bucle
hecho
```

y podemos aplicarlo a nuestro ejemplo de esta manera:

```bash
$ for filename in basilisk.dat minotaur.dat unicorn.dat
> hacer
>     echo $filename
>     head -n 2 $filename | tail -n 1
> hecho
```

```output
basilisk.dat
CLASIFICACIÓN: basiliscus vulgaris
minotaur.dat
CLASIFICACIÓN: bos hominus
unicorn.dat
CLASIFICACIÓN: equus monoceros
```

::::::::::::::::::::::::::::::::::::::::: destacado

## Sigue el indicio

El símbolo de la shell cambia de `$` a `>` y viceversa mientras estábamos
escribiendo nuestro bucle. El segundo indicio, `>`, es diferente para recordarnos
que no hemos terminado de escribir un comando completo todavía. Se puede usar un punto y coma, ';',
para separar dos comandos escritos en una sola línea.


::::::::::::::::::::::::::::::::::::::::::::::::::

Cuando la shell ve la palabra clave `for`,
sabe que debe repetir un comando (o grupo de comandos) una vez por cada elemento en una lista.
Cada vez que se ejecuta el bucle (llamado iteración), se le asigna un elemento de la lista
a la **variable** en secuencia, y los comandos dentro del bucle se ejecutan antes de pasar
al siguiente elemento de la lista.
Dentro del bucle,
llamamos al valor de la variable poniendo `$` delante.
El `$` le indica al intérprete de la shell que trate
la variable como un nombre de variable y sustituya su valor en su lugar,
en lugar de tratarlo como texto o como un comando externo.

En este ejemplo, la lista son tres nombres de archivo: `basilisk.dat`, `minotaur.dat`, y `unicorn.dat`.
Cada vez que se itera el bucle, primero usamos `echo` para imprimir el valor que la variable
`$filename` tiene actualmente. Esto no es necesario para el resultado, pero beneficioso para nosotros aquí para
seguir más fácilmente. A continuación, asignaremos un nombre de archivo a la variable `filename`
y ejecutaremos el comando `head`.
La primera vez que se ejecutó el bucle,
`$filename` es `basilisk.dat`.
El intérprete ejecuta el comando `head` en `basilisk.dat`
y envía las dos primeras líneas al comando `tail`,
que luego imprime la segunda línea de `basilisk.dat`.
Para la segunda iteración, `$filename` se convierte en
`minotaur.dat`. Esta vez, la shell ejecuta `head` en `minotaur.dat`
y envía las dos primeras líneas al comando `tail`,
que luego imprime la segunda línea de `minotaur.dat`.
Para la tercera iteración, `$filename` se convierte en
`unicorn.dat`, por lo que la shell ejecuta el comando `head` en ese archivo,
y `tail` en la salida de eso.
Dado que la lista solo tenía tres elementos, la shell sale del bucle `for`.

::::::::::::::::::::::::::::::::::::::::: destacado

## Mismos símbolos, significados diferentes

Aquí vemos `>` que se utiliza como un indicador de shell, mientras que `>` también
se utiliza para redirigir la salida.
De manera similar, `$` se utiliza como un indicador de shell, pero, como vimos anteriormente,
también se utiliza para solicitar a la shell que obtenga el valor de una variable.

Si la *shell* imprime `>` o `$`, entonces espera que escribas algo,
y el símbolo es un indicador.

Si *tú* escribes `>` o `$` tú mismo, es una instrucción tuya de que
la shell debe redirigir la salida o obtener el valor de una variable.


::::::::::::::::::::::::::::::::::::::::::::::::::

Al utilizar variables, también es
posible colocar los nombres entre llaves para delimitar claramente el nombre de la variable:
`$filename` es equivalente a `${filename}`, pero es diferente de
`${file}name`. Es posible que encuentres esta notación en los programas de otras personas.

Hemos llamado a la variable de este bucle `filename`
para que su propósito sea más claro para los lectores humanos.
A la propia shell no le importa cómo se llame la variable;
si escribimos este bucle como:

```bash
$ for x in basilisk.dat minotaur.dat unicorn.dat
> hacer
>     head -n 2 $x | tail -n 1
> hecho
```

o:

```bash
$ for temperature in basilisk.dat minotaur.dat unicorn.dat
> hacer
>     head -n 2 $temperature | tail -n 1
> hecho
```

Funcionaría exactamente de la misma manera.
*No hagas esto.*
Los programas solo son útiles si las personas pueden entenderlos,
por lo que los nombres sin sentido (como `x`) o los nombres engañosos (como `temperature`)
aumentan las posibilidades de que el programa no haga lo que sus lectores piensan que hace.

En los ejemplos anteriores, las variables (`thing`, `filename`, `x` y `temperature`)
podrían haber tenido cualquier otro nombre, siempre que tenga un significado tanto para la persona
que escribe el código como para la persona que lo lee.

También hay que tener en cuenta que los bucles se pueden usar para otras cosas que no sean nombres de archivo, como una lista de números
o un subconjunto de datos.

::::::::::::::::::::::::::::::::::::::: desafío

## Escribe tu propio bucle

¿Cómo escribirías un bucle que muestre los 10 números del 0 al 9?

::::::::::::::: solución

## Solución

```bash
$ for loop_variable in 0 1 2 3 4 5 6 7 8 9
> hacer
>     echo $loop_variable
> hecho
```

```output
0
1
2
3
4
5
6
7
8
9
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: desafío

## Variables en Bucles

Este ejercicio se refiere al directorio `shell-lesson-data/exercise-data/alkanes`.
`ls *.pdb` muestra el siguiente resultado:

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

¿Cuál es el resultado del siguiente código?

```bash
$ for datafile in *.pdb
> hacer
>     ls *.pdb
> hecho
```

Ahora, ¿cuál es el resultado del siguiente código?

```bash
$ for datafile in *.pdb
> hacer
>     ls $datafile
> hecho
```

¿Por qué estos dos bucles producen resultados diferentes?

::::::::::::::: solución

## Solución

El primer bloque de código produce el mismo resultado en cada iteración del bucle.
Bash expande el comodín *.pdb dentro del cuerpo del bucle (así como
antes de que el bucle comience) para que coincida con todos los archivos que terminan en .pdb,
y luego los lista usando `ls`.
El bucle expandido se vería así:

```bash
$ for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> hacer
>     ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> hecho
```

```output
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
```

El segundo bloque de código lista un archivo diferente en cada iteración.
El valor de la variable `datafile` se evalúa usando `$datafile`,
y luego se lista usando `ls`.


```output
cubane.pdb
ethane.pdb
methane.pdb
octane.pdb
pentane.pdb
propane.pdb
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: desafío

## Limitar Conjuntos de Archivos

¿Cuál sería la salida de ejecutar el siguiente bucle en el
directorio `shell-lesson-data/exercise-data/alkanes`?

```bash
$ for filename in c*
> hacer
>     ls $filename
> hecho
```

1. No se enumeran los archivos.
2. Se enumeran todos los archivos.
3. Solo se enumeran `cubane.pdb`, `octane.pdb` y `pentane.pdb`.
4. Solo se enumera `cubane.pdb`.

::::::::::::::: solución

## Solución

La opción correcta es 4. `*` coincide con cero o más caracteres, por lo que se
coincidirán los nombres de archivo con cero o más caracteres antes de la letra c y cero o más caracteres
después de la letra c.


:::::::::::::::::::::::::

¿Cómo se diferenciaría la salida de usar este comando en su lugar?

```bash
$ for filename in *c*
> hacer
>     ls $filename
> hecho
```

1. Se enumerarían los mismos archivos.
2. Se enumeran todos los archivos esta vez.
3. No se enumeran archivos esta vez.
4. Se enumerarían los archivos `cubane.pdb` y `octane.pdb`.
5. Solo se enumera el archivo `octane.pdb`.

::::::::::::::: solución

## Solución

La opción correcta es 4. `*` coincide con cero o más caracteres, por lo que se coincidirá con un nombre de archivo con cero o más
caracteres antes de la letra c y cero o más caracteres después de la letra c.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: desafío

## Guardando en un archivo en un Bucle - Parte Uno

En el directorio `shell-lesson-data/exercise-data/alkanes`, ¿cuál es el efecto de este bucle?

```bash
for alkanes in *.pdb
do
    echo $alkanes
    cat $alkanes > alkanes.pdb
done
```

1. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y
   `pentane.pdb`, y el texto de `pentane.pdb` se guardará en un archivo llamado `alkanes.pdb`.
2. Imprime `cubane.pdb`, `ethane.pdb` y `methane.pdb`, y el texto de los tres archivos
   se concatenaría y se guardaría en un archivo llamado `alkanes.pdb`.
3. Imprime `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y `pentane.pdb`,
   y el texto de `pentane.pdb` se guardará en un archivo llamado `alkanes.pdb`.
4. Ninguna de las anteriores.

::::::::::::::: solución

## Solución

La opción correcta es 1. El texto de cada archivo se escribirá en el archivo `alkanes.pdb` uno tras otro.
Sin embargo, el archivo se sobrescribirá en cada iteración del bucle, por lo que el contenido final de
`alkanes.pdb`
es el texto del archivo `pentane.pdb`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::: desafío

## Guardando en un archivo en un Bucle - Parte Dos

También en el directorio `shell-lesson-data/exercise-data/alkanes`,
¿cuál sería la salida del siguiente bucle?

```bash
for datafile in *.pdb
do
    cat $datafile >> all.pdb
done
```

1. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y
   `pentane.pdb` se concatenaría y se guardaría en un archivo llamado `all.pdb`.
2. Se guardará el texto de `ethane.pdb` en un archivo llamado `all.pdb`.
3. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y `pentane.pdb`
   se concatenaría y se imprimiría en la pantalla y se guardaría en un archivo llamado `all.pdb`.

4. Todo el texto de `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb` y `pentane.pdb`
   se imprimirá en la pantalla y se guardará en un archivo llamado `all.pdb`.

::::::::::::::: solución

## Solución

3 es la respuesta correcta. `>>` anexa a un archivo, en lugar de sobrescribirlo con la salida redirigida de un comando.
Dado que la salida redirigida no produce ningún resultado, es difícil verificar
que el bucle esté funcionando correctamente. Sin embargo, aprendimos antes cómo imprimir cadenas
usando `echo`, y podemos modificar el bucle para usar `echo` y así imprimir nuestros comandos sin
ejecutarlos realmente. Como tal, podemos verificar qué comandos *se ejecutarían* en el
bucle no modificado.

El siguiente diagrama
muestra lo que sucede cuando se ejecuta el bucle modificado y demuestra cómo
el uso juicioso de `echo` es una buena técnica de depuración.

![](fig/shell_script_for_loop_flow_chart.svg){alt='El bucle "for datafile in .pdb; do echo cp $datafile original-$datafile;done" asignará sucesivamente el nombre de todos los archivos ".pdb" en su directorio actual a la variable "$datafile" y luego ejecutará el comando do con el nombre del archivo como parámetro. Con los archivos "basilisk.dat", "minotaur.dat" y "unicorn.dat" en el directorio actual, el bucle invocará sucesivamente el comando echo tres veces e imprimirá tres líneas: "cp ###basislisk.dat original-basilisk.dat", luego "cp minotaur.datoriginal-minotaur.dat" y, finalmente "cp unicorn.datoriginal-unicorn.dat"'}

## El Proceso de Nelle: Procesando Archivos

Nelle está lista ahora para procesar sus archivos de datos utilizando `goostats.sh` ---
un script de shell escrito por su supervisor. Esto calcula algunas estadísticas a partir de un archivo de muestra de proteínas y toma dos argumentos:

1. un archivo de entrada (que contiene los datos en bruto)
2. un archivo de salida (para almacenar las estadísticas calculadas)

Como aún está aprendiendo cómo usar la shell,
ella decide construir los comandos requeridos en etapas.
Su primer paso es asegurarse de que pueda seleccionar los archivos de entrada correctos: recuerda,
estos son aquellos cuyos nombres terminan en 'A' o 'B', en lugar de 'Z'.
Dirigiéndose al directorio `north-pacific-gyre`, Nelle escribe:

```bash
$ cd
$ cd Escritorio/shell-lesson-data/north-pacific-gyre
$ for datafile in NENE*A.txt NENE*B.txt
> hacer
>     echo $datafile
> hecho
```

```output
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
```

Su próximo paso es decidir cómo llamar a los archivos que el programa de análisis `goostats.sh` creará.
Agregar el prefijo de cada nombre de archivo de entrada con 'stats' parece simple,
así que ella modifica su bucle para hacer eso:

```bash
$ for datafile in NENE*A.txt NENE*B.txt
> hacer
>     echo $datafile stats-$datafile
> hecho
```

```output
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
```

Aún no ha ejecutado `goostats.sh`,
pero ahora está segura de que puede seleccionar los archivos correctos y generar los nombres de salida correctos.

Escribir comandos una y otra vez se está volviendo tedioso,
sin embargo,
y Nelle está preocupada por cometer errores,
por lo que en lugar de volver a ingresar su bucle,
presiona <kbd>↑</kbd>.
Como respuesta,
la shell muestra nuevamente el bucle completo en una línea
(usando puntos y coma para separar las partes):

```bash
$ for datafile in NENE*A.txt NENE*B.txt; hacer echo $datafile stats-$datafile; hecho
```

Usando <kbd>←</kbd>,
Nelle navega hacia el comando `echo` y lo cambia a `bash goostats.sh`:

```bash
$ for datafile in NENE*A.txt NENE*B.txt; hacer bash goostats.sh $datafile stats-$datafile; hecho
```

Cuando presiona <kbd>Enter</kbd>,
la shell ejecuta el comando modificado.
Sin embargo, no parece suceder nada, no hay salida.
Después de un momento, Nelle se da cuenta de que como su script ya no imprime nada en la pantalla,
no tiene idea de si se está ejecutando, y mucho menos de qué tan rápido.
Detiene el comando en ejecución escribiendo <kbd>Ctrl</kbd>\+<kbd>C</kbd>,
usa <kbd>↑</kbd> para repetir el comando,
y lo edita para que diga:

```bash
$ for datafile in NENE*A.txt NENE*B.txt; hacer echo $datafile;
bash goostats.sh $datafile stats-$datafile; hecho
```

::::::::::::::::::::::::::::::::::::::::: destacado

## Principio y fin

Podemos movernos al principio de una línea en la shell escribiendo <kbd>Ctrl</kbd>\+<kbd>A</kbd>
y al final usando <kbd>Ctrl</kbd>\+<kbd>E</kbd>.


::::::::::::::::::::::::::::::::::::::::::::::::::

Cuando ejecuta su programa ahora,
produce una línea de salida cada cinco segundos aproximadamente:

```output
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
```

1518 veces 5 segundos,
dividido por 60,
le indica que su script tomará aproximadamente dos horas en ejecutarse.
Como última comprobación,
abre otra ventana de terminal,
ingresa al directorio `north-pacific-gyre`,
y usa `cat stats-NENE01729B.txt`
para examinar uno de los archivos de salida.
Se ve bien,
así que decide tomar un café y ponerse al día con su lectura.

::::::::::::::::::::::::::::::::::::::::: destacado

## Quien Conoce la Historia Puede Elegir Repetirla

Otra forma de repetir trabajos anteriores es usar el comando `history`
para obtener una lista de los últimos cientos de comandos que se han ejecutado y
luego usar `!123` (donde '123' se reemplaza por el número del comando) para
repetir uno de esos comandos. Por ejemplo, si Nelle escribe esto:

```bash
$ history | tail -n 5
```

```output
456  for datafile in NENE*A.txt NENE*B.txt; hacer   echo $datafile stats-$datafile; hecho
457  for datafile in NENE*A.txt NENE*B.txt; hacer echo $datafile stats-$datafile; hecho
458  for datafile in NENE*A.txt NENE*B.txt; hacer bash goostats.sh $datafile stats-$datafile; hecho
459  for datafile in NENE*A.txt NENE*B.txt; hacer echo $datafile; bash goostats.sh $datafile
stats-$datafile; hecho
460  history | tail -n 5
```

entonces puede volver a ejecutar `goostats.sh` en los archivos simplemente escribiendo
`!459`.


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: destacado

## Otros Comandos de Historia

Hay varios otros comandos rápidos para acceder a la historia.

- <kbd>Ctrl</kbd>\+<kbd>R</kbd> entra en el modo de búsqueda de la historia 'reverse-i-search' y encuentra el
  comando más reciente en tu historial que coincida con el texto que escribas a continuación.
  Presiona <kbd>Ctrl</kbd>\+<kbd>R</kbd> una o más veces adicionales para buscar coincidencias anteriores.
  Luego puedes usar las teclas de flecha izquierda y derecha para elegir esa línea y editarla, luego presionar <kbd>Return</kbd> para ejecutar el comando.
- `!!` recupera el comando inmediatamente anterior
  (puedes encontrar esto más conveniente o no en comparación con usar <kbd>↑</kbd>)
- `!$` recupera la última palabra del último comando.
  Eso es útil con más frecuencia de lo que podrías pensar: después
  de `bash goostats.sh NENE01729B.txt stats-NENE01729B.txt`, puedes escribir
  `less !$` para ver el archivo `stats-NENE01729B.txt`, lo cual es
  más rápido que usar <kbd>↑</kbd> y editar la línea de comandos.



::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: destacado

## Autocompletar

Si presionas <kbd>Tab</kbd> después de escribir el inicio de un nombre de archivo o directorio, la shell intentará completar automáticamente el nombre por ti. Si hay varias opciones posibles, presionar <kbd>Tab</kbd> dos veces mostrará todas las opciones, para que puedas escribir o seleccionar la correcta.


::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::: destacado

## Historia de la Shell

Para ver y buscar tu historial de comandos en un archivo, en lugar de en la memoria, puedes agregar el siguiente par de líneas a tu archivo `~/.bash_profile` o `~/.bashrc`:

```bash
export HISTFILESIZE=5000  # Esto conservará los últimos 5000 comandos de tu historial.
export HISTSIZE=2000      # Esto te permitirá escribir los últimos 2000 comandos desde tu historial.
export HISTCONTROL=ignoredups  # Esto evita que comandos duplicados se escriban en tu historial.
export HISTTIMEFORMAT="%Y-%m-%d %T "  # Esto te muestra la fecha y la marca de tiempo en tu historial.
shopt -s histappend    # Esto asegura que el historial se mantendrá cuando cierres una terminal y abra otra.
```

Estos comandos se ejecutarán automáticamente al abrir una nueva ventana de terminal o al iniciar sesión de nuevo en una máquina remota.