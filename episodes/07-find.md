---
title: Encontrando cosas
teaching: 25
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Usar `grep` para seleccionar líneas de archivos de texto que coincidan con patrones simples.
- Usar `find` para encontrar archivos y directorios cuyos nombres coincidan con patrones simples.
- Usar la salida de un comando como argumento(s) de línea de comandos para otro comando.
- Explicar qué se entiende por archivos 'de texto' y 'binarios', y por qué muchas herramientas comunes no manejan bien estos últimos.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Cómo puedo encontrar archivos?
- ¿Cómo puedo encontrar cosas en archivos?

::::::::::::::::::::::::::::::::::::::::::::::::::

De la misma manera en que muchos de nosotros ahora usamos 'Google' como un verbo que significa 'buscar', los programadores de Unix a menudo usan la palabra 'grep'.
'grep' es una contracción de 'global/regular expression/print', una secuencia común de operaciones en los editores de texto tempranos de Unix.
También es el nombre de un programa muy útil en la línea de comandos.

`grep` encuentra e imprime líneas en archivos que coinciden con un patrón.
Para nuestros ejemplos, utilizaremos un archivo que contiene tres haikus tomados de una competencia de [1998](https://web.archive.org/web/19991201042211/http://salon.com/21st/chal/1998/01/26chal.html) en la revista *Salon* (Crédito a los autores Bill Torcaso, Howard Korder y Margaret Segall, respectivamente. Ver [Página 1](https://web.archive.org/web/20000310061355/http://www.salon.com/21st/chal/1998/02/10chal2.html) y [Página 2](https://web.archive.org/web/20000229135138/http://www.salon.com/21st/chal/1998/02/10chal3.html) para los poemas haiku archivados). Para este conjunto de ejemplos, vamos a trabajar en el subdirectorio de escritura:

```bash
$ cd
$ cd Escritorio/datos-leccion-shell/ejercicio-datos/escritura
$ cat haiku.txt
```

```output
El Tao que se ve
No es el verdadero Tao, hasta que
Traigas tóner fresco.

Con la búsqueda viene la pérdida
y la presencia de la ausencia:
"Mi tesis" no encontrada.

Ayer funcionaba
Hoy no está funcionando
Así es el software.
```

Encontremos las líneas que contienen la palabra 'no':

```bash
$ grep no haiku.txt
```

```output
No es el verdadero Tao, hasta que
"Mi tesis" no encontrada
Hoy no está funcionando
```

Aquí, `no` es el patrón que estamos buscando.
El comando grep busca en el archivo, buscando coincidencias con el patrón especificado.
Para usarlo, escribe `grep`, luego el patrón que estás buscando y finalmente
el nombre del archivo (o archivos) en los que estás buscando.

La salida son las tres líneas en el archivo que contienen las letras 'no'.

Por defecto, grep busca un patrón de manera sensible a mayúsculas y minúsculas.
Además, el patrón de búsqueda que hemos seleccionado no tiene que formar una palabra completa,
como veremos en el siguiente ejemplo.

Busquemos el patrón: 'El'.

```bash
$ grep El haiku.txt
```

```output
El Tao que se ve
"Mi tesis" no encontrada.
```

Esta vez, se muestran dos líneas que incluyen las letras 'El',
una de las cuales contenía nuestro patrón de búsqueda dentro de una palabra más larga, 'tesis'.

Para restringir las coincidencias a las líneas que contienen la palabra 'El' por sí sola,
podemos usar la opción `-w` de `grep`.
Esto limitará las coincidencias a los límites de las palabras.

Más adelante en esta lección, también veremos cómo podemos cambiar el comportamiento de búsqueda de grep con respecto a la sensibilidad a mayúsculas y minúsculas.

```bash
$ grep -w El haiku.txt
```

```output
El Tao que se ve
```

Ten en cuenta que un 'límite de palabra' incluye el inicio y el final de una línea, así que no
solo letras rodeadas por espacios.
A veces no queremos buscar una sola palabra, sino una frase. También podemos hacer esto con `grep`
poniendo la frase entre comillas.

```bash
$ grep -w "no está" haiku.txt
```

```output
Hoy no está funcionando
```

Ahora hemos visto que no necesitas usar comillas alrededor de palabras individuales,
pero es útil usar comillas al buscar múltiples palabras.
También ayuda a distinguir entre el término o frase de búsqueda
y el archivo que está siendo buscado.
Usaremos comillas en los ejemplos restantes.

Otra opción útil es `-n`, que enumera las líneas que coinciden:

```bash
$ grep -n "está" haiku.txt
```

```output
3:El Tao que se ve
11:Hoy no está funcionando
```

Aquí, podemos ver que las líneas 3 y 11 contienen las letras 'está'.

Podemos combinar opciones (es decir, indicadores) de la misma manera que hacemos con otros comandos de Unix.
Por ejemplo, encontremos las líneas que contienen la palabra 'el'.
Podemos combinar la opción `-w` para encontrar las líneas que contienen la palabra 'el'
y `-n` para enumerar las líneas que coinciden:

```bash
$ grep -n -w "el" haiku.txt
```

```output
```

Ahora queremos usar la opción `-i` para que nuestra búsqueda no distinga mayúsculas de minúsculas:

```bash
$ grep -n -w -i "el" haiku.txt
```

```output
3:El Tao que se ve
11:Hoy no está funcionando
```

Ahora, queremos usar la opción `-v` para invertir nuestra búsqueda, es decir, queremos mostrar
las líneas que no contienen la palabra 'el'.

```bash
$ grep -n -w -v "el" haiku.txt
```

```output
1:El Tao que se ve
2:No es el verdadero Tao, hasta que
4:Traigas tóner fresco.
5:
6:Con la búsqueda viene la pérdida
7:y la presencia de la ausencia:
8:"Mi tesis" no encontrada.
9:
10:Ayer funcionaba
11:Hoy no está funcionando
12:Así es el software.
```

Si usamos la opción `-r` (recursiva),
`grep` puede buscar en forma recursiva un patrón en un conjunto de archivos en subdirectorios.

Busquemos de forma recursiva 'Ayer' en el directorio `datos-leccion-shell/ejercicio-datos/escritura`:

```bash
$ grep -r Ayer .
```

```output
./LittleWomen.txt:"Ayer, cuando tía estaba dormida y yo estaba tratando de quedarme quieta como una
./LittleWomen.txt:Ayer en la cena, cuando un oficial austriaco nos miró y luego
./LittleWomen.txt:Pasé ayer un día tranquilo enseñando, cosiendo y escribiendo en mi
./haiku.txt:Ayer funcionaba
```

`grep` tiene muchas otras opciones. Para saber cuáles son, puedes escribir:

```bash
$ grep --help
```

```output
Uso: grep [OPCIÓN]... PATRÓN [ARCHIVO]...
Busca en cada ARCHIVO (o entrada estándar) el PATRÓN.
El PATRÓN es, por defecto, una expresión regular básica (BRE).
Ejemplo: grep -i 'hola mundo' menu.h main.c

Selección e interpretación de expresiones regulares:
  -E, --extended-regexp     el PATRÓN es una expresión regular extendida (ERE)
  -F, --fixed-strings       el PATRÓN es un conjunto de cadenas fijas separadas por líneas nuevas
  -G, --basic-regexp        el PATRÓN es una expresión regular básica (BRE)
  -P, --perl-regexp         el PATRÓN es una expresión regular de Perl
  -e, --regexp=PATRÓN       usa PATRÓN para la búsqueda
  -f, --file=ARCHIVO        extrae PATRÓN de ARCHIVO
  -i, --ignore-case         ignora las distinciones entre mayúsculas y minúsculas
  -w, --word-regexp         fuerza a que el PATRÓN coincida solo con palabras completas
  -x, --line-regexp         fuerza a que el PATRÓN coincida solo con líneas completas
  -z, --null-data           una línea de datos termina en un byte 0, no en nueva línea

Miscellaneous:
...        ...        ...
```

:::::::::::::::::::::::::::::::::::::::::  desafío

## Usando `grep`

¿Qué comando daría como resultado la siguiente salida?

```output
y la presencia de la ausencia:
```

1. `grep "de" haiku.txt`
2. `grep -E "de" haiku.txt`
3. `grep -w "de" haiku.txt`
4. `grep -i "de" haiku.txt`

:::::::::::::::  solución

## Solución

La respuesta correcta es la 3, porque la opción `-w` busca solo coincidencias de palabras completas.
Las otras opciones también coincidirán con 'de' cuando forme parte de otra palabra.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

En los comandos anteriores, utilizamos `grep` para encontrar líneas en archivos de texto que coincidieran con un patrón.
Ahora, veamos cómo usar el comando `find` para encontrar los propios archivos.
Nuevamente, tiene muchas opciones;
para mostrar cómo funciona las más sencillas, utilizaremos la siguiente estructura de directorio dentro de `datos-leccion-shell/ejercicio-datos`.

```output
.
├── animal-counts/
│   └── animals.csv
├── creatures/
│   ├── basilisk.dat
│   ├── minotaur.dat
│   └── unicorn.dat
├── numbers.txt
├── alkanes/
│   ├── cubane.pdb
│   ├── ethane.pdb
│   ├── methane.pdb
│   ├── octane.pdb
│   ├── pentane.pdb
│   └── propane.pdb
└── writing/
    ├── haiku.txt
    └── LittleWomen.txt
```

El directorio `ejercicio-datos` contiene un archivo, `numbers.txt`, y cuatro directorios:
`animal-counts`, `creatures`, `alkanes` y `writing`, que contienen varios archivos.

Para nuestro primer comando, vamos a ejecutar `find .` (recuerda ejecutar este comando desde la carpeta `datos-leccion-shell/ejercicio-datos`).

```bash
$ find .
```

```output
.
./writing
./writing/LittleWomen.txt
./writing/haiku.txt
./creatures
./creatures/basilisk.dat
./creatures/unicorn.dat
./creatures/minotaur.dat
./animal-counts
./animal-counts/animals.csv
./numbers.txt
./alkanes
./alkanes/ethane.pdb
./alkanes/propane.pdb
./alkanes/octane.pdb
./alkanes/pentane.pdb
./alkanes/methane.pdb
./alkanes/cubane.pdb
```

Como siempre, el `.` por sí solo se refiere al directorio de trabajo actual,
que es donde queremos que comience nuestra búsqueda.
La salida de `find` son los nombres de todos los archivos **y** directorios
bajo el directorio de trabajo actual.
Esto puede parecer inútil al principio, pero `find` tiene muchas opciones
para filtrar la salida y en esta lección descubriremos algunas de ellas.

La primera opción en nuestra lista es
`-type d` que significa 'cosas que son directorios'.
Efectivamente, la salida de `find` son los nombres de los cinco directorios (incluido `.`):

```bash
$ find . -type d
```

```output
.
./writing
./creatures
./animal-counts
./alkanes
```

Observa que los objetos que `find` encuentra no se enumeran en un orden específico.
Si cambiamos `-type d` a `-type f`,
obtenemos una lista de todos los archivos en su lugar:

```bash
$ find . -type f
```

```output
./writing/LittleWomen.txt
./writing/haiku.txt
./creatures/basilisk.dat
./creatures/unicorn.dat
./creatures/minotaur.dat
./animal-counts/animals.csv
./numbers.txt
./alkanes/ethane.pdb
./alkanes/propane.pdb
./alkanes/octane.pdb
./alkanes/pentane.pdb
./alkanes/methane.pdb
./alkanes/cubane.pdb
```

Ahora intentemos hacer coincidir por nombre:

```bash
$ find . -name *.txt
```

```output
./numbers.txt
```

Esperábamos que encontrara todos los archivos de texto,
pero solo imprime `./numbers.txt`.
El problema es que el shell expande caracteres comodín como `*` *antes* de ejecutarse los comandos.
Dado que `*.txt` en el directorio actual se expande a `./numbers.txt`,
el comando que realmente ejecutamos fue:

```bash
$ find . -name numbers.txt
```

`find` hizo lo que le pedimos; simplemente le pedimos lo incorrecto.

Para obtener lo que queremos,
hagamos lo que hicimos con `grep`:
poner `*.txt` entre comillas para evitar que el shell expanda el carácter comodín `*`.
De esta manera,
`find` obtiene el patrón `*.txt`, no el nombre de archivo expandido `numbers.txt`:

```bash
$ find . -name "*.txt"
```

```output
./writing/LittleWomen.txt
./writing/haiku.txt
./numbers.txt
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Listando vs. Encontrando

`ls` y `find` pueden hacer cosas similares dados los indicadores correctos,
pero en circunstancias normales,
`ls` lista todo lo que puede,
mientras que `find` busca cosas que tienen ciertas propiedades y las muestra.


::::::::::::::::::::::::::::::::::::::::::::::::::

Como dijimos antes,
el poder de la línea de comandos radica en combinar herramientas.
Hemos visto cómo hacerlo con tuberías (pipes);
veamos otra técnica.
Como acabamos de ver,
`find . -name "*.txt"` nos da una lista de todos los archivos de texto en o debajo del directorio actual.
¿Cómo podemos combinar eso con `wc -l` para contar las líneas de todos esos archivos?

La forma más sencilla es poner el comando `find` dentro de `$()`:

```bash
$ wc -l $(find . -name "*.txt")
```

```output
  21022 ./writing/LittleWomen.txt
     11 ./writing/haiku.txt
      5 ./numbers.txt
  21038 total
```

Cuando el shell ejecuta este comando,
lo primero que hace es ejecutar lo que está dentro de `$()`.
Luego, reemplaza la expresión `$()` con la salida de ese comando.
Dado que la salida de `find` son los tres nombres de archivo `./writing/LittleWomen.txt`, `./writing/haiku.txt` y `./numbers.txt`, el shell construye el comando:

```bash
$ wc -l ./writing/LittleWomen.txt ./writing/haiku.txt ./numbers.txt
```

que es lo que queríamos.
Esta expansión es exactamente lo que hace el shell cuando expande caracteres comodín como `*` y `?`,
pero nos permite usar cualquier comando que queramos como nuestro propio 'comodín'.

Es muy común usar `find` y `grep` juntos.
El primero encuentra archivos que coinciden con un patrón;
el segundo busca líneas dentro de esos archivos que coincidan con otro patrón.
Aquí, por ejemplo, podemos encontrar archivos de texto que contienen la palabra "búsqueda"
buscando la cadena 'búsqueda' en todos los archivos `.txt` en el directorio actual:

```bash
$ grep "búsqueda" $(find . -name "*.txt")
```

```output
./writing/LittleWomen.txt:Pude verla sentada en el escalón superior, preocupada por encontrar su libro, pero estaba
./writing/haiku.txt:Con la búsqueda viene la pérdida
```

:::::::::::::::::::::::::::::::::::::::  desafío

## Coincidencias y Restas

La opción `-v` de `grep` invierte la búsqueda de patrones, de modo que solo se imprimen las líneas
que *no* coinciden con el patrón. Dada esta información, ¿cuál de los siguientes comandos encontrará todos los archivos `.dat` en `creatures`
excepto `unicorn.dat`?
Una vez que hayas pensado en tu respuesta, puedes probar los comandos en el directorio
`datos-leccion-shell/ejercicio-datos`.

1. `find creatures -name "*.dat" | grep -v unicorn`
2. `find creatures -name *.dat | grep -v unicorn`
3. `grep -v "unicorn" $(find creatures -name "*.dat")`
4. Ninguna de las anteriores.

:::::::::::::::  solución

## Solución

La opción 1 es correcta. Poner la expresión de coincidencia entre comillas evita que el shell
la expanda, por lo que se pasa al comando `find`.

La opción 2 también funciona en este caso porque el shell intenta expandir `*.dat`,
pero no hay archivos `*.dat` en el directorio actual,
por lo que la expresión comodín se pasa a `find`.
Primero encontramos esto en
[episodio 3](03-create.md).

La opción 3 es incorrecta porque busca en el contenido de los archivos las líneas que
no coinciden con 'unicorn', en lugar de buscar en los nombres de archivo.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Archivos binarios

Nos hemos centrado exclusivamente en encontrar patrones en archivos de texto. ¿Qué ocurre si
tus datos se almacenan como imágenes, en bases de datos o en algún otro formato?

Un puñado de herramientas amplía `grep` para manejar algunos formatos no textuales. Pero un
enfoque más generalizable es convertir los datos a texto o extraer los elementos similares a texto de los datos. Por un lado, hace que las cosas sencillas sean fáciles de hacer. Por otro lado, las cosas complejas suelen ser imposibles. Por
ejemplo, es bastante sencillo escribir un programa que extraiga las dimensiones X e Y
de archivos de imagen para que `grep` pueda jugar con ellas, pero ¿cómo escribirías algo para encontrar valores en una hoja de cálculo cuyas celdas contengan fórmulas?

Una última opción es reconocer que la línea de comandos y el procesamiento de texto tienen
sus límites, y usar otro lenguaje de programación.
Cuando llegue el momento de hacer esto, no seas demasiado duro con la línea de comandos. Muchos
lenguajes de programación modernos han tomado muchas
ideas de ella, y la imitación también es la forma más sincera de elogio.


::::::::::::::::::::::::::::::::::::::::::::::::::

La shell de Unix tiene más años que la mayoría de las personas que la usan. Ha
sobrevivido tanto tiempo porque es uno de los entornos de programación más productivos que se hayan creado, tal vez incluso *el* más productivo. Su sintaxis puede resultar críptica, pero las personas que la han dominado pueden experimentar con diferentes comandos de manera interactiva, y luego usar lo que han aprendido para automatizar su trabajo. Las interfaces gráficas de usuario pueden ser más fáciles de usar al principio, pero una vez que se aprenden, la productividad en la shell es inigualable. Y como escribió Alfred North Whitehead en 1911, 'La civilización avanza mediante la ampliación del número de operaciones importantes que podemos realizar sin pensar en ellas'.

:::::::::::::::::::::::::::::::::::::::  desafío

## Comprensión de lectura de la tubería `find`

Escribe un breve comentario explicando el siguiente script de shell:

```shell
wc -l $(find . -name "*.dat") | sort -n
```

:::::::::::::::  solución

## Solución

1. Encuentra todos los archivos con extensión `.dat` de forma recursiva desde el directorio actual.
2. Cuenta el número de líneas que contienen cada uno de estos archivos.
3. Ordena la salida del paso 2 de forma numérica.
  
  
  

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- `find` encuentra archivos con propiedades específicas que coinciden con patrones.
- `grep` selecciona líneas en archivos que coinciden con patrones.
- `--help` es una opción compatible con muchos comandos de bash y programas que se pueden ejecutar desde Bash, para mostrar más información sobre cómo usar estos comandos o programas.
- `man [comando]` muestra la página del manual de un comando dado.
- `$([comando])` inserta la salida de un comando en su lugar.