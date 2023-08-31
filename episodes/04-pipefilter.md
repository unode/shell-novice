---

título: Tuberías y filtros
enseñanza: 25
ejercicios: 10
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Redirigir la salida de un comando a un archivo.
- Construir tuberías de comandos con dos o más etapas.
- Explicar lo que suele suceder si un programa o una tubería no recibe ninguna entrada para procesar.
- Explicar la ventaja de vincular comandos con tuberías y filtros.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Cómo puedo combinar comandos existentes para hacer cosas nuevas?

::::::::::::::::::::::::::::::::::::::::::::::::::

Ahora que conocemos algunos comandos básicos,
finalmente podemos ver la característica más poderosa del shell:
la facilidad con la que nos permite combinar programas existentes de nuevas formas.
Comenzaremos con el directorio `shell-lesson-data/exercise-data/alkanes`
que contiene seis archivos que describen algunas moléculas orgánicas simples.
La extensión `.pdb` indica que estos archivos están en formato de banco de datos de proteínas,
un formato de texto simple que especifica el tipo y la posición de cada átomo en la molécula.

```bash
$ ls
```

```output
cubane.pdb    methane.pdb    pentane.pdb
ethane.pdb    octane.pdb     propane.pdb
```

Vamos a ejecutar un ejemplo de comando:

```bash
$ wc cubane.pdb
```

```output
20  156 1158 cubane.pdb
```

`wc` es el comando 'word count':
cuenta el número de líneas, palabras y caracteres en los archivos (devolviendo los valores
en ese orden de izquierda a derecha).

Si ejecutamos el comando `wc *.pdb`, el `*` en `*.pdb` coincide con cero o más caracteres,
por lo que el shell convierte `*.pdb` en una lista de todos los archivos `.pdb` en el directorio actual:

```bash
$ wc *.pdb
```

```output
  20  156  1158  cubane.pdb
  12  84   622   ethane.pdb
   9  57   422   methane.pdb
  30  246  1828  octane.pdb
  21  165  1226  pentane.pdb
  15  111  825   propane.pdb
 107  819  6081  total
```

Tenga en cuenta que `wc *.pdb` también muestra el número total de todas las líneas en la última línea de la salida.

Si ejecutamos `wc -l` en lugar de solo `wc`,
la salida muestra solo el número de líneas por archivo:

```bash
$ wc -l *.pdb
```

```output
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
```

Las opciones `-m` y `-w` también se pueden usar con el comando `wc` para mostrar
solo el número de caracteres o el número de palabras, respectivamente.

:::::::::::::::::::::::::::::::::::::::::  callout

## ¿Por qué no está haciendo nada?

¿Qué sucede si se supone que un comando debe procesar un archivo, pero no le damos un nombre de archivo? Por ejemplo, ¿qué sucede si escribimos:

```bash
$ wc -l
```

pero no escribimos `*.pdb` (o cualquier otra cosa) después del comando? Como no tiene ningún nombre de archivo, `wc` asume que se supone que debe procesar la entrada proporcionada en el símbolo del sistema, por lo que simplemente se queda allí y espera a que le demos algunos datos de manera interactiva. Sin embargo, desde el exterior, todo lo que vemos es que está allí, y el comando no parece hacer nada.

Si comete este tipo de error, puede salir de este estado manteniendo presionada la tecla de control (<kbd>Ctrl</kbd>) y presionando la tecla <kbd>C</kbd> una vez: <kbd>Ctrl</kbd>\+<kbd>C</kbd>. Luego suelte ambas teclas.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Capturando la salida de los comandos

¿Qué archivos contienen la menor cantidad de líneas?
Es una pregunta fácil de responder cuando solo hay seis archivos,
¿pero qué sucede si hubiera 6000?
Nuestro primer paso hacia una solución es ejecutar el comando:

```bash
$ wc -l *.pdb > lengths.txt
```

El símbolo mayor que, `>`, le indica al shell que **redirija** la salida del comando a un archivo en lugar de imprimirlo en la pantalla. Este comando no imprime salida en la pantalla, porque todo lo que `wc` habría impreso ha ido al archivo `lengths.txt` en su lugar. Si el archivo no existe antes de emitir el comando, el shell creará el archivo. Si el archivo ya existe, se sobrescribirá silenciosamente, lo que puede provocar pérdida de datos. Por lo tanto, los comandos de **redirección** requieren precaución.

`ls lengths.txt` confirma que el archivo existe:

```bash
$ ls lengths.txt
```

```output
lengths.txt
```

Ahora podemos enviar el contenido de `lengths.txt` a la pantalla usando `cat lengths.txt`. El comando `cat` recibe su nombre de 'concatenar', es decir, unir, e imprime el contenido de los archivos uno tras otro. En este caso, solo hay un archivo, por lo que `cat` nos muestra lo que contiene:

```bash
$ cat lengths.txt
```

```output
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Página de salida por página

Continuaremos usando `cat` en esta lección, por conveniencia y consistencia, pero tiene la desventaja de que siempre muestra todo el archivo en su pantalla. Lo más útil en la práctica es el comando `less` (p. ej., `less lengths.txt`). Esto muestra un pantalla completa del archivo y luego se detiene. Puede avanzar una pantalla presionando la barra espaciadora o retroceder una página presionando `b`. Presione `q` para salir.


::::::::::::::::::::::::::::::::::::::::::::::::::

## Filtrado de la salida

A continuación, utilizaremos el comando `sort` para ordenar el contenido del archivo `lengths.txt`. Pero primero, haremos un ejercicio para aprender un poco sobre el comando `sort`:

:::::::::::::::::::::::::::::::::::::::  challenge

## ¿Qué hace `sort -n`?

El archivo `shell-lesson-data/exercise-data/numbers.txt` contiene las siguientes líneas:

```source
10
2
19
22
6
```

Si ejecutamos `sort` en este archivo, la salida es:

```output
10
19
2
22
6
```

Si ejecutamos `sort -n` en el mismo archivo, obtenemos esto en su lugar:

```output
2
6
10
19
22
```

Explique por qué `-n` tiene este efecto.

:::::::::::::::  solution

## Solución

La opción `-n` especifica una clasificación numérica en lugar de una clasificación alfanumérica.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

También usaremos la opción `-n` para especificar que la clasificación es numérica en lugar de alfanumérica. Esto no cambia el archivo; en su lugar, envía el resultado clasificado a la pantalla:

```bash
$ sort -n lengths.txt
```

```output
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
```

Podemos poner la lista ordenada de líneas en otro archivo temporal llamado `sorted-lengths.txt` poniendo `> sorted-lengths.txt` después del comando, al igual que usamos `> lengths.txt` para poner la salida de `wc` en `lengths.txt`. Una vez que hayamos hecho eso, podemos ejecutar otro comando llamado `head` para obtener las primeras líneas en `sorted-lengths.txt`:

```bash
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
```

```output
  9  methane.pdb
```

Usar `-n 1` con `head` le indica que solo queremos la primera línea del archivo; `-n 20` obtendría las primeras 20, y así sucesivamente. Dado que `sorted-lengths.txt` contiene las longitudes de nuestros archivos ordenados de menor a mayor, la salida de `head` debe ser el archivo con menos líneas.

:::::::::::::::::::::::::::::::::::::::::  callout

## Redirigir al mismo archivo

Es muy mala idea intentar redirigir
la salida de un comando que opera en un archivo
al mismo archivo. Por ejemplo:

```bash
$ sort -n lengths.txt > lengths.txt
```

Hacer algo como esto puede dar resultados incorrectos y/o eliminar el contenido de `lengths.txt`.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ¿Qué significa `>>`?

Hemos visto el uso de `>`, pero también hay un operador similar `>>` que funciona de manera ligeramente diferente. Aprenderemos sobre las diferencias entre estos dos operadores imprimiendo algunas cadenas. Podemos usar el comando `echo` para imprimir cadenas, por ejemplo:

```bash
$ echo El comando echo imprime texto
```

```output
El comando echo imprime texto
```

Ahora prueba los comandos a continuación para revelar la diferencia entre los dos operadores:

```bash
$ echo hello > testfile01.txt
```

y:

```bash
$ echo hello >> testfile02.txt
```

Sugerencia: Intente ejecutar cada comando dos veces seguidas y luego examine los archivos de salida.

:::::::::::::::  solution

## Solución

En el primer ejemplo con `>`, se escribe la cadena 'hello' en `testfile01.txt`, pero el archivo se sobrescribe cada vez que se ejecuta el comando.

Vemos en el segundo ejemplo que el operador `>>` también escribe 'hello' en un archivo (en este caso, `testfile02.txt`), pero agrega la cadena al archivo si ya existe (es decir, cuando lo ejecutamos por segunda vez).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Añadiendo datos

Ya hemos visto el comando `head`, que imprime líneas desde el comienzo de un archivo. `tail` es similar, pero imprime líneas desde el final de un archivo en su lugar.

Considera el archivo `shell-lesson-data/exercise-data/animal-counts/animals.csv`. Después de estos comandos, seleccionar la respuesta que corresponda al archivo `animals-subset.csv`:

```bash
$ head -n 3 animals.csv > animals-subset.csv
$ tail -n 2 animals.csv >> animals-subset.csv
```

1. Las primeras tres líneas de `animals.csv`
2. Las dos últimas líneas de `animals.csv`
3. Las primeras tres líneas y las dos últimas líneas de `animals.csv`
4. Las líneas segunda y tercera de `animals.csv`

:::::::::::::::  solution

## Solución

La opción 3 es correcta.
Para que la opción 1 sea correcta, solo ejecutaríamos el comando `head`.
Para que la opción 2 sea correcta, solo ejecutaríamos el comando `tail`.
Para que la opción 4 sea correcta, tendríamos que enviar la salida de `head` al comando `tail -n 2` usando un conducto, lo que haríamos `head -n 3 animals.csv | tail -n 2 > animals-subset.csv`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Pasando la salida a otro comando

En nuestro ejemplo de encontrar el archivo con menos líneas, estamos utilizando dos archivos intermedios `lengths.txt` y `sorted-lengths.txt` para almacenar la salida. Esta es una forma confusa de trabajar porque incluso cuando comprende lo que hacen `wc`, `sort` y `head`, esos archivos intermedios dificultan seguir lo que está sucediendo. Podemos facilitar su comprensión ejecutando `sort` y `head` juntos:

```bash
$ sort -n lengths.txt | head -n 1
```

```output
  9  methane.pdb
```

La barra vertical `|` entre los dos comandos se llama **tubería**. Le indica al shell que queremos usar la salida del comando de la izquierda como entrada para el comando de la derecha.

Esto ha eliminado la necesidad del archivo `sorted-lengths.txt`.

## Combinar múltiples comandos

Nada nos impide encadenar tuberías consecutivamente. Podemos, por ejemplo, enviar la salida de `wc` directamente a `sort`, y luego enviar la salida resultante a `head`. Esto elimina la necesidad de cualquier archivo intermedio.

Comenzaremos usando una tubería para enviar la salida de `wc` a `sort`:

```bash
$ wc -l *.pdb | sort -n
```

```output
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
```

Luego podemos enviar esa salida a través de otra tubería, a `head`, para que el pipeline completo sea:

```bash
$ wc -l *.pdb | sort -n | head -n 1
```

```output
   9  methane.pdb
```

Esto es exactamente como un matemático anidando funciones como *log(3x)* y diciendo 'el logaritmo de tres veces *x*'. En nuestro caso, el algoritmo es 'la cabeza de la clasificación de la cuenta de líneas de `*.pdb`'.

La redirección y las tuberías utilizadas en los últimos comandos se ilustran a continuación:

![](fig/redirects-and-pipes.svg){alt='Redirects and Pipes of different commands: "wc -l \*.pdb" will direct theoutput to the shell. "wc -l \*.pdb > lengths" will direct output to the file"lengths". "wc -l \*.pdb | sort -n | head -n 1" will build a pipeline where theoutput of the "wc" command is the input to the "sort" command, the output ofthe "sort" command is the input to the "head" command and the output of the"head" command is directed to the shell'}


:::::::::::::::::::::::::::::::::::::::  challenge

## Uniendo comandos con una tubería

En nuestro directorio actual, queremos encontrar los 3 archivos que tienen la menor cantidad de líneas. ¿Qué comando de la siguiente lista funcionaría?

1. `wc -l * > sort -n > head -n 3`
2. `wc -l * | sort -n | head -n 1-3`
3. `wc -l * | head -n 3 | sort -n`
4. `wc -l * | sort -n | head -n 3`

:::::::::::::::  solution

## Solución

La opción 4 es la solución.
El carácter de tubería `|` se usa para conectar la salida de un comando con la entrada de otro.
`>` se usa para redirigir la salida estándar a un archivo.
¡Pruébalo en el directorio `shell-lesson-data/exercise-data/alkanes`!



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Herramientas diseñadas para trabajar juntas

Esta idea de vincular programas es la razón por la que Unix ha tenido tanto éxito. En lugar de crear programas enormes que intentan hacer muchas cosas diferentes, los programadores de Unix se centran en crear muchas herramientas simples que cada una hace bien una tarea y que funcionan bien entre sí. Este modelo de programación se llama 'tuberías y filtros'. Ya hemos visto las tuberías; un **filtro** es un programa como `wc` o `sort` que transforma un flujo de entrada en un flujo de salida. Casi todas las herramientas estándar de Unix pueden funcionar de esta manera. A menos que se indique lo contrario, leen de la entrada estándar, hacen algo con lo que han leído y escriben en la salida estándar.

La clave está en que cualquier programa que lea líneas de texto de la entrada estándar y escriba líneas de texto en la salida estándar se puede combinar con cualquier otro programa que se comporte de esta manera. Puede *y debe* escribir sus programas de esta manera para que usted y otras personas puedan combinar esos programas en tuberías para multiplicar su poder.

:::::::::::::::::::::::::::::::::::::::  challenge

## Comprensión de la lectura de la tubería

Un archivo llamado `animals.csv` (en la carpeta `shell-lesson-data/exercise-data/animal-counts`) contiene los siguientes datos:

```source
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
2012-11-06,deer,2
2012-11-06,fox,4
2012-11-07,rabbit,16
2012-11-07,bear,1
```

¿Qué texto pasa por cada una de las tuberías y la redirección final en la canalización a continuación?
Nota, el comando `sort -r` clasifica en orden inverso.

```bash
$ cat animals.csv | head -n 5 | tail -n 3 | sort -r > final.txt
```

Sugerencia: construya la canalización paso a paso para probar su comprensión

:::::::::::::::  solution

## Solución

El comando `head` extrae las primeras 5 líneas de `animals.csv`. Luego, las últimas 3 líneas se extraen de las anteriores 5 utilizando el comando `tail`. Con el comando `sort -r`, esas 3 líneas se ordenan en orden inverso. Finalmente, la salida se redirige a un archivo: `final.txt`. El contenido de este archivo se puede verificar ejecutando `cat final.txt`. El archivo debe contener las siguientes líneas:

```source
2012-11-06,rabbit,19
2012-11-06,deer,2
2012-11-05,raccoon,7
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Tubería de construcción

Para el archivo `animals.csv` del ejercicio anterior, considera el siguiente comando:

```bash
$ cut -d , -f 2 animals.csv
```

El comando `cut` se utiliza para eliminar o 'cortar' ciertas secciones de cada línea en el archivo, y `cut` espera que las líneas estén separadas en columnas por un carácter de <kbd>Tab</kbd>. Un carácter utilizado de esta manera se llama un **delimitador**. En el ejemplo anterior, usamos la opción `-d` para especificar que la coma es nuestro carácter delimitador. También hemos usado la opción `-f` para especificar que queremos extraer el segundo campo (columna). Esto nos da la siguiente salida:

```output
deer
rabbit
raccoon
rabbit
deer
fox
rabbit
bear
```

El comando `uniq` filtra las líneas coincidentes adyacentes en un archivo. ¿Cómo podrías ampliar esta tubería (usando `uniq` y otro comando) para descubrir qué animales contiene el archivo (sin duplicados en sus nombres)?

:::::::::::::::  solution

## Solución

```bash
$ cut -d , -f 2 animals.csv | sort | uniq
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## ¿Qué tubería?

El archivo `animals.csv` contiene 8 líneas de datos formateados de la siguiente manera:

```output
2012-11-05,deer,5
2012-11-05,rabbit,22
2012-11-05,raccoon,7
2012-11-06,rabbit,19
...
```

El comando `uniq` tiene una opción `-c` que da un recuento de la cantidad de veces que una línea ocurre en su entrada. Suponiendo que su directorio actual es `shell-lesson-data/exercise-data/animal-counts`, ¿qué comando usaría para producir una tabla que muestre el recuento total de cada tipo de animal en el archivo?

1. `sort animals.csv | uniq -c`
2. `sort -t, -k2,2 animals.csv | uniq -c`
3. `cut -d, -f 2 animals.csv | uniq -c`
4. `cut -d, -f 2 animals.csv | sort | uniq -c`
5. `cut -d, -f 2 animals.csv | sort | uniq -c | wc -l`

:::::::::::::::  solution

## Solución

La opción 4 es la respuesta correcta. Si tiene dificultades para entender por qué, intente ejecutar los comandos o subsecciones de las canalizaciones (asegúrese de estar en el directorio `shell-lesson-data/exercise-data/animal-counts`).



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::


## La canalización de Nelle: verificación de archivos

Nelle ha ejecutado sus muestras a través de las máquinas de ensayo y ha creado 17 archivos en el directorio `north-pacific-gyre` descrito anteriormente. Como verificación rápida, comenzando desde el directorio `shell-lesson-data`, Nelle escribe:

```bash
$ cd north-pacific-gyre
$ wc -l *.txt
```

La salida es 18 líneas como esta:

```output
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
```

Ahora escribe esto:

```bash
$ wc -l *.txt | sort -n | head -n 5
```

```output
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
```

Ups: uno de los archivos tiene 60 líneas menos que los demás. Cuando va a verificarlo, ve que hizo ese ensayo a las 8:00 del lunes por la mañana: es probable que alguien haya estado usando la máquina durante el fin de semana y ella olvidó restablecerla. Antes de ejecutar nuevamente esa muestra, verifica si algún archivo tiene demasiados datos:

```bash
$ wc -l *.txt | sort -n | tail -n 5
```

```output
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
```

Esos números parecen estar bien, pero ¿qué hace esa 'Z' allí en la antepenúltima línea? Todas sus muestras deberían estar marcadas con 'A' o 'B'; por convención, su laboratorio utiliza 'Z' para indicar muestras con información faltante. Para encontrar otros casos como este, ella hace esto:

```bash
$ ls *Z.txt
```

```output
NENE01971Z.txt    NENE02040Z.txt
```

Efectivamente, cuando revisa el registro en su computadora portátil, no hay registro de la profundidad para ninguno de esos dos muestras. Dado que es demasiado tarde para obtener la información de otra manera, debe excluir esos dos archivos de su análisis. Podría eliminarlos con `rm`, pero en realidad hay algunos análisis que podría hacer más adelante en los que la profundidad no importa, por lo que en su lugar, debe tener cuidado más adelante para seleccionar los archivos utilizando las expresiones de comodín `NENE*A.txt` y `NENE*B.txt`.

:::::::::::::::::::::::::::::::::::::::  challenge

## Eliminando archivos innecesarios

Supongamos que deseas eliminar tus archivos de datos procesados y solo mantener tus archivos sin procesar y el script de procesamiento para ahorrar espacio de almacenamiento. Los archivos sin procesar tienen la extensión `.dat` y los archivos procesados tienen la extensión `.txt`. ¿Cuál de las siguientes opciones eliminaría todos los archivos de datos procesados y *solo* los archivos de datos procesados?

1. `rm ?.txt`
2. `rm *.txt`
3. `rm * .txt`
4. `rm *.*`

:::::::::::::::  solution

## Solución

1. Esto eliminaría los archivos `.txt` con nombres de un solo carácter.
2. Esta es la respuesta correcta
3. El shell expandiría `*` para que coincida con todo en el directorio actual, por lo que el comando intentaría eliminar todos los archivos coincidentes y un archivo adicional llamado `.txt`
4. El shell expande `*.*` para que coincida con todos los nombres de archivo que contienen al menos un `.`, incluidos los archivos procesados (`.txt`) *y* los archivos sin procesar (`.dat`)


:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- `wc` cuenta líneas, palabras y caracteres en su entrada.
- `cat` muestra el contenido de su entrada.
- `sort` ordena su entrada.
- `head` muestra las 10 primeras líneas de su entrada.
- `tail` muestra las 10 últimas líneas de su entrada.
- `command > [file]` redirige la salida de un comando a un archivo (sobrescribiendo cualquier contenido existente).
- `command >> [file]` agrega la salida de un comando a un archivo.
- `[primero] | [segundo]` es una tubería: la salida del primer comando se utiliza como entrada para el segundo.
- La mejor manera de usar el shell es usar tuberías para combinar programas simples de un solo propósito (filtros).

::::::::::::::::::::::::::::::::::::::::::::::::::