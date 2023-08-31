---
title: Scripts de Shell
teaching: 30
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Escribir un script de shell que ejecute un comando o una serie de comandos para un conjunto fijo de archivos.
- Ejecutar un script de shell desde la línea de comandos.
- Escribir un script de shell que opere en un conjunto de archivos definidos por el usuario en la línea de comandos.
- Crear tuberías que incluyan scripts de shell que tú y otros hayan escrito.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Cómo puedo guardar y reutilizar comandos?

::::::::::::::::::::::::::::::::::::::::::::::::::

Finalmente, estamos listos para ver lo que hace que el shell sea un entorno de programación tan poderoso.
Vamos a tomar los comandos que repetimos frecuentemente y guardarlos en archivos
para que podamos ejecutar todas esas operaciones de nuevo más tarde escribiendo un solo comando.
Por razones históricas,
un conjunto de comandos guardados en un archivo se llama normalmente un **script de shell**,
pero no te equivoques --- estos son en realidad pequeños programas.

Escribir scripts de shell no sólo hará tu trabajo más rápido, sino que también evitarás tener que volver a escribir
los mismos comandos una y otra vez. También hará que sea más preciso (menos posibilidades de
cometer errores de escritura) y más reproducible. Si vuelves a tu trabajo más tarde (o si alguien más encuentra
tu trabajo y quiere trabajar en él), podrás reproducir los mismos resultados simplemente
ejecutando tu script, en lugar de tener que recordar o volver a escribir una larga lista de comandos.

Empecemos volviendo a `alkanes/` y creando un nuevo archivo, `middle.sh`, que será
nuestro script de shell:

```bash
$ cd alkanes
$ nano middle.sh
```

El comando `nano middle.sh` abre el archivo `middle.sh` en el editor de texto 'nano'
(que se ejecuta dentro del shell).
Si el archivo no existe, se creará.
Podemos usar el editor de texto para editar directamente el archivo insertando la siguiente línea:

```source
head -n 15 octane.pdb | tail -n 5
```

Esta es una variación de la tubería que construimos anteriormente, que selecciona las líneas 11-15 de
el archivo `octane.pdb`. Recuerda, no lo estamos ejecutando como un comando justo aún;
sólo estamos incorporando los comandos en un archivo.

Luego guardamos el archivo (`Ctrl-O` en nano) y salimos del editor de texto (`Ctrl-X` en nano).
Comprueba que el directorio `alkanes` ahora contiene un archivo llamado `middle.sh`.

Una vez que hemos guardado el archivo,
podemos pedirle al shell que ejecute los comandos que contiene.
Nuestro shell se llama `bash`, así que ejecutamos el siguiente comando:

```bash
$ bash middle.sh
```

```output
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

Efectivamente,
la salida de nuestro script es exactamente lo que obtendríamos si ejecutáramos esa tubería directamente.

:::::::::::::::::::::::::::::::::::::::::  callout

## Texto vs. lo que sea

Normalmente llamamos programas como Microsoft Word o LibreOffice Writer "editores de texto",
pero debemos ser un poco más cuidadosos cuando se trata de programación. Por defecto, Microsoft Word utiliza archivos `.docx` para almacenar no
sólo texto, sino también información de formato sobre fuentes, encabezados, etc. Esta información adicional no se almacena como caracteres y no significa
nada para las herramientas como `head`, que espera que los archivos de entrada contengan sólo las letras, dígitos y puntuación de un teclado estándar de computadora. Al editar programas, por lo tanto, debes usar un editor de texto plano o asegurarte de guardar los archivos como texto plano.


::::::::::::::::::::::::::::::::::::::::::::::::::

¿Y si queremos seleccionar líneas de un archivo arbitrario?
Podríamos editar `middle.sh` cada vez para cambiar el nombre del archivo,
pero eso probablemente llevaría más tiempo que escribir el comando de nuevo
en el shell y ejecutarlo con un nuevo nombre de archivo.
En su lugar, vamos a editar `middle.sh` y hacerlo más versátil:

```bash
$ nano middle.sh
```

Ahora, dentro de "nano", reemplaza el texto `octane.pdb` por la variable especial llamada `$1`:

```source
head -n 15 "$1" | tail -n 5
```

Dentro de un script de shell,
`$1` significa 'el primer nombre de archivo (u otro argumento) en la línea de comandos'.
Ahora podemos ejecutar nuestro script de esta manera:

```bash
$ bash middle.sh octane.pdb
```

```output
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
```

o en un archivo diferente de esta manera:

```bash
$ bash middle.sh pentane.pdb
```

```output
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Comillas dobles alrededor de los argumentos

Por la misma razón por la que ponemos la variable del bucle encerrada entre comillas dobles,
en caso de que el nombre del archivo contenga espacios,
rodeamos `$1` con comillas dobles.


::::::::::::::::::::::::::::::::::::::::::::::::::

Actualmente, necesitamos editar `middle.sh` cada vez que queremos ajustar el rango de
líneas que se devuelve.
Arreglemos eso configurando nuestro script para que en su lugar use tres argumentos en la línea de comandos.
Después del primer argumento en la línea de comandos (`$1`), cada argumento adicional que proporcionemos estará accesible a través de las variables especiales `$1`, `$2`, `$3`,
que se referirán al primer, segundo, tercer argumento en la línea de comandos, respectivamente.

Sabido esto, podemos usar argumentos adicionales para definir el rango de líneas que
se pasará a `head` y `tail`, respectivamente:

```bash
$ nano middle.sh
```

```source
head -n "$2" "$1" | tail -n "$3"
```

Ahora podemos ejecutar:

```bash
$ bash middle.sh pentane.pdb 15 5
```

```output
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
```

cambiando los argumentos de nuestro comando, podemos cambiar el comportamiento de nuestro script:

```bash
$ bash middle.sh pentane.pdb 20 5
```

```output
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
```

Esto funciona,
pero la próxima persona que lea `middle.sh` puede tardar un momento en comprender qué hace.
Podemos mejorar nuestro script agregando algunos **comentarios** en la parte superior:

```bash
$ nano middle.sh
```

```source
# Seleccionar líneas del centro de un archivo.
# Uso: bash middle.sh nombre_archivo fin_linea num_lineas
head -n "$2" "$1" | tail -n "$3"
```

Un comentario comienza con el carácter `#` y se extiende hasta el final de la línea.
La computadora ignora los comentarios,
pero son invaluables para ayudar a las personas (incluyéndote a ti mismo en el futuro) a comprender y utilizar los scripts.
El único inconveniente es que cada vez que modifiques el script,
debes asegurarte de que el comentario siga siendo preciso. Una explicación que dirige
al lector en la dirección incorrecta es peor que ninguna en absoluto.

¿Y si queremos procesar muchos archivos en una sola tubería?
Por ejemplo, si queremos ordenar nuestros archivos `.pdb` por longitud, escribiríamos:

```bash
$ wc -l *.pdb | sort -n
```

porque `wc -l` enumera el número de líneas en los archivos
(recordemos que `wc` significa 'word count', agregando la opción `-l` significa 'contar líneas' en su lugar)
y `sort -n` los ordena de forma numérica.
Podríamos poner esto en un archivo,
pero sólo ordenaría una lista de archivos `.pdb` en el directorio actual.
Si queremos ser capaces de obtener una lista ordenada de otros tipos de archivos,
necesitamos una forma de obtener todos esos nombres en el script.
No podemos usar `$1`, `$2`, etc.
porque no sabemos cuántos archivos hay.
En su lugar, utilizamos la variable especial `$@`,
que significa
'Todos los argumentos de línea de comandos del script de shell'.
También debemos colocar `$@` entre comillas dobles
para manejar el caso de los argumentos que contengan espacios
(`"$@"` es una sintaxis especial y es equivalente a `"$1"` `"$2"` ...).

Aquí tienes un ejemplo:

```bash
$ nano sorted.sh
```

```source
# Ordenar archivos por su longitud.
# Uso: bash sorted.sh uno_o_más_nombres_de_archivo
wc -l "$@" | sort -n
```

```bash
$ bash sorted.sh *.pdb ../creatures/*.dat
```

```output
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/minotaur.dat
163 ../creatures/unicorn.dat
596 total
```

:::::::::::::::::::::::::::::::::::::::  challenge

## List Unique Species

Leah tiene varios cientos de archivos de datos, cada uno de los cuales tiene el siguiente formato:

```source
2013-11-05,deer,5
2013-11-05,rabbit,22
2013-11-05,raccoon,7
2013-11-06,rabbit,19
2013-11-06,deer,2
2013-11-06,fox,1
2013-11-07,rabbit,18
2013-11-07,bear,1
```

Se proporciona un ejemplo de este tipo de archivo en
`shell-lesson-data/exercise-data/animal-counts/animals.csv`.

Podemos usar el comando `cut -d , -f 2 animals.csv | sort | uniq` para producir
las especies únicas en `animals.csv`.
Para evitar tener que escribir esta serie de comandos cada vez,
un científico puede optar por escribir un script de shell en su lugar.

Escribe un script de shell llamado `species.sh` que tome cualquier número de
nombres de archivos como argumentos de línea de comandos y use una variación del comando anterior
para imprimir una lista de las especies únicas que aparecen en cada uno de esos archivos por separado.

:::::::::::::::  solution

## Solución

```bash
# Script para encontrar las especies únicas en archivos csv donde la especie es el segundo campo de datos
# Este script acepta cualquier número de nombres de archivo como argumentos de línea de comandos

# Bucle sobre todos los archivos
for file in $@
do
    echo "Especies únicas en $file:"
    # Extraer nombres de especies
    cut -d , -f 2 $file | sort | uniq
done
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

Supongamos que acabamos de ejecutar una serie de comandos que hicieron algo útil --- por ejemplo,
creando un gráfico que nos gustaría utilizar en un artículo.
Nos gustaría poder recrear el gráfico más tarde si es necesario,
así que queremos guardar los comandos en un archivo.
En lugar de volver a escribirlos
(y potencialmente equivocarnos),
podemos hacer esto:

```bash
$ history | tail -n 5 > redo-figure-3.sh
```

El archivo `redo-figure-3.sh` ahora contiene:

```source
297 bash goostats.sh NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff.sh stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
```

Después de un momento de trabajo en un editor para eliminar los números de serie de los comandos,
y para eliminar la última línea donde llamamos al comando `history`,
tenemos un registro completamente preciso de cómo creamos ese gráfico.

:::::::::::::::::::::::::::::::::::::::  challenge

## Why Record Commands in the History Before Running Them?

If you run the command:

```bash
$ history | tail -n 5 > recent.sh
```

the last command in the file is the `history` command itself, i.e.,
the shell has added `history` to the command log before actually
running it. In fact, the shell *always* adds commands to the log
before running them. Why do you think it does this?

:::::::::::::::  solution

## Solution

If a command causes something to crash or hang, it might be useful
to know what that command was, in order to investigate the problem.
Were the command only be recorded after running it, we would not
have a record of the last command run in the event of a crash.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

En la práctica, la mayoría de las personas desarrollan scripts de shell ejecutando comandos
en el indicador de shell unas cuantas veces
para asegurarse de que están haciendo lo correcto,
y luego los guardan en un archivo para reutilizarlos.
Este estilo de trabajo permite a las personas reciclar
lo que descubren sobre sus datos y su flujo de trabajo con una sola llamada a `history`
y un poco de edición para limpiar la salida
y guardarla como un script de shell.

## El pipeline de Nelle: Creando un script

El supervisor de Nelle insistió en que todos sus análisis debían ser reproducibles.
La forma más fácil de capturar todos los pasos es en un script.

Primero volvemos al directorio de proyectos de Nelle:

```bash
$ cd ../../north-pacific-gyre/
```

Crea un archivo usando `nano` ...

```bash
$ nano do-stats.sh
```

...que contiene lo siguiente:

```bash
# Calcula estadísticas para los archivos de datos.
for datafile in "$@"
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

Lo guarda en un archivo llamado `do-stats.sh`
para que ahora pueda rehacer la primera etapa de su análisis escribiendo:

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt
```

También puede hacer esto:

```bash
$ bash do-stats.sh NENE*A.txt NENE*B.txt | wc -l
```

para que la salida sea sólo el número de archivos procesados
en lugar de los nombres de los archivos que se procesaron.

Una cosa a tener en cuenta sobre el script de Nelle es que
permite a la persona que lo ejecuta decidir qué archivos procesar.
Podría haberlo escrito así:

```bash
# Calcula estadísticas para los archivos de los sitios A y B.
for datafile in NENE*A.txt NENE*B.txt
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
```

La ventaja es que esto siempre selecciona los archivos correctos:
no tienes que recordar excluir los archivos 'Z'.
La desventaja es que *siempre* selecciona sólo esos archivos --- no se puede ejecutar en todos los archivos
(incluyendo los archivos 'Z'),
o en los archivos 'G' o 'H' que están produciendo sus colegas en la Antártida,
sin editar el script. Si quisiera ser más aventurera,
podría modificar su script para comprobar los argumentos de la línea de comandos,
y usar `NENE*A.txt NENE*B.txt` si no se proporcionan ninguno.
Por supuesto, esto introduce otro compromiso entre flexibilidad y complejidad.

:::::::::::::::::::::::::::::::::::::::  challenge

## Variables in Shell Scripts

En el directorio `alkanes`, imagina que tienes un script de shell llamado `script.sh` que contiene los siguientes comandos:

```bash
head -n $2 $1
tail -n $3 $1
```

Mientras estás en el directorio `alkanes`, escribes el siguiente comando:

```bash
$ bash script.sh '*.pdb' 1 1
```

¿Qué salida esperarías ver?

1. Todas las líneas entre la primera y la última línea de cada archivo que termina en `.pdb` en el directorio `alkanes`
2. La primera y la última línea de cada archivo que termina en `.pdb` en el directorio `alkanes`
3. La primera y la última línea de cada archivo en el directorio `alkanes`
4. Un error debido a las comillas alrededor de `*.pdb`

:::::::::::::::  solution

## Solución

La respuesta correcta es la 2.

Las variables especiales `$1`, `$2` y `$3` representan los argumentos de línea de comandos dados al
script, de manera que los comandos ejecutados son:

```bash
$ head -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
$ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
```

El shell no expande `'*.pdb'` porque está entre comillas.
Como tal, el primer argumento del script es `'*.pdb'` que se expande dentro del
script por `head` y `tail`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Encontrar el archivo más largo con una extensión dada

Escribe un script de shell llamado `longest.sh` que tome el nombre de un
directorio y una extensión de archivos como argumentos, e imprima
el nombre del archivo con más líneas en ese directorio
con esa extensión. Por ejemplo:

```bash
$ bash longest.sh shell-lesson-data/exercise-data/alkanes pdb
```

imprimiría el nombre del archivo `.pdb` en `shell-lesson-data/exercise-data/alkanes` que tiene
más líneas.

Siéntete libre de probar tu script en otro directorio, por ejemplo:

```bash
$ bash longest.sh shell-lesson-data/exercise-data/writing txt
```

:::::::::::::::  solution

## Solución

```bash
# Script de shell que toma dos argumentos:
#    1. un nombre de directorio
#    2. una extensión de archivo
# e imprime el nombre del archivo en ese directorio
# con más líneas que coincide con la extensión de archivo.

wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
```

La primera parte de la tubería, `wc -l $1/*.$2 | sort -n`, cuenta
las líneas en cada archivo y las ordena numéricamente (mayor al final). Cuando
hay más de un archivo, `wc` también muestra una línea de resumen final,
que da el número total de líneas en *todos* los archivos. Usamos `tail -n 2 | head -n 1` para descartar esta última línea.

Con `wc -l $1/*.$2 | sort -n | tail -n 1` veremos la línea de resumen final:
podemos construir nuestra tubería en piezas para asegurarnos de entender
la salida.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Script Reading Comprehension

Para esta pregunta, considera una vez más el directorio `shell-lesson-data/exercise-data/alkanes`.
Esto contiene varios archivos `.pdb` además de cualquier otro archivo que
puedas haber creado.
Explica qué harían cada uno de los siguientes tres scripts cuando se ejecutan como
`bash script1.sh *.pdb`, `bash script2.sh *.pdb` y `bash script3.sh *.pdb`, respectivamente.

```bash
# Script 1
echo *.*
```

```bash
# Script 2
for filename in $1 $2 $3
do
    cat $filename
done
```

```bash
# Script 3
echo $@.pdb
```

:::::::::::::::  solution

## Soluciones

En cada caso, el shell expande el comodín en `*.pdb` antes de pasar la lista resultante de nombres de archivo como argumentos al script.

El Script 1 imprimiría una lista de todos los archivos que contienen un punto en su nombre.
Los argumentos pasados al script no se utilizan en ningún lugar del script.

El Script 2 imprimiría el contenido de los primeros 3 archivos con la extensión .pdb.
`$1`, `$2` y `$3` se refieren al primer, segundo y tercer argumento respectivamente.

El Script 3 imprimiría todos los argumentos del script (es decir, todos los archivos .pdb),
seguidos de `.pdb`.
`$@` se refiere a *todos* los argumentos dados a un script de shell.

```output
cubane.pdb ethane.pdb methane.pdb octane.pdb pentane.pdb propane.pdb.pdb
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Debugging Scripts

Supongamos que has guardado el siguiente script en un archivo llamado `do-errors.sh`
en el directorio `north-pacific-gyre` de Nelle:

```bash
# Calculate stats for data files.
for datafile in "$@"
do
    echo $datfile
    bash goostats.sh $datafile stats-$datafile
done
```

Cuando lo ejecutas desde el directorio `north-pacific-gyre`:

```bash
$ bash do-errors.sh NENE*A.txt NENE*B.txt
```

la salida está en blanco.
Para averiguar por qué, vuelve a ejecutar el script usando la opción `-x`:

```bash
$ bash -x do-errors.sh NENE*A.txt NENE*B.txt
```

¿Qué te muestra la salida?
¿Qué línea es responsable del error?

:::::::::::::::  solution

## Solución

La opción `-x` hace que `bash` se ejecute en modo de depuración.
Esto imprime cada comando a medida que se ejecuta, lo que ayudará a ubicar errores.
En este ejemplo, podemos ver que `echo` no imprime nada. Hemos cometido un error tipográfico
en el nombre de la variable del bucle, y la variable `datfile` no existe, por lo que devuelve
una cadena vacía.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Guarda los comandos en archivos (normalmente llamados scripts de shell) para su reutilización.
- `bash [nombre_de_archivo]` ejecuta los comandos guardados en un archivo.
- `$@` se refiere a todos los argumentos de línea de comandos de un script de shell.
- `$1`, `$2`, etc., se refieren al primer argumento de línea de comandos, al segundo argumento de línea de comandos, etc.
- Coloca las variables entre comillas si los valores pueden contener espacios.
- Permitir a los usuarios decidir qué archivos procesar es más flexible y más consistente con los comandos internos de Unix.

::::::::::::::::::::::::::::::::::::::::::::::::::