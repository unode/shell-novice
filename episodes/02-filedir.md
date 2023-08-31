---
title: Navegando entre Archivos y Directorios
teaching: 30
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Explicar las similitudes y diferencias entre un archivo y un directorio.
- Traducir una ruta absoluta en una ruta relativa y viceversa.
- Construir rutas absolutas y relativas que identifiquen archivos y directorios específicos.
- Usar opciones y argumentos para cambiar el comportamiento de un comando de terminal.
- Demostrar el uso de la autocompletación y explicar sus ventajas.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Cómo puedo navegar en mi computadora?
- ¿Cómo puedo ver qué archivos y directorios tengo?
- ¿Cómo puedo especificar la ubicación de un archivo o directorio en mi computadora?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: instructor

Introducir y navegar por el sistema de archivos en la terminal
(cubiertos en la sección [Navegando por archivos y directorios](02-filedir.md))
puede ser confuso. Los alumnos pueden tener una terminal y un explorador de archivos
abiertos lado a lado para que puedan ver el contenido y la estructura de archivos
mientras usan la terminal para navegar por el sistema.

::::::::::::::::::::::::::::::::::::::::::::::::::

La parte del sistema operativo responsable de administrar archivos y directorios
se llama **sistema de archivos**.
Organiza nuestros datos en archivos,
que contienen información,
y directorios (también llamados 'carpetas'),
que contienen archivos u otros directorios.

Varios comandos se utilizan con frecuencia para crear, inspeccionar, renombrar y eliminar archivos y directorios.
Para comenzar a explorarlos, vamos a nuestra ventana abierta de terminal.

Primero, descubramos dónde estamos ejecutando un comando llamado `pwd`
(que significa "imprimir directorio de trabajo actual"). Los directorios son como *lugares* - en cualquier momento
mientras usamos la terminal, estamos en un lugar exactamente llamado
nuestro **directorio de trabajo actual**. Los comandos leen y escriben principalmente archivos en el
directorio de trabajo actual, es decir, 'aquí', por lo que es importante saber dónde estamos antes de ejecutar
un comando. `pwd` te muestra dónde estás:

```bash
$ pwd
```

```output
/Users/nelle
```

Aquí,
la respuesta de la computadora es `/Users/nelle`,
que es el **directorio de inicio** de Nelle:

:::::::::::::::::::::::::::::::::::::::::  callout

## Variación del directorio de inicio

La ruta del directorio de inicio se verá diferente en diferentes sistemas operativos.
En Linux, puede verse como `/home/nelle`,
y en Windows, será similar a `C:\Documents and Settings\nelle` o
`C:\Users\nelle`.
(Tenga en cuenta que puede verse ligeramente diferente para diferentes versiones de Windows).
En futuros ejemplos, hemos utilizado la salida de Mac como predeterminada: Linux y Windows
la salida puede diferir ligeramente, pero debería ser generalmente similar.

También asumiremos que tu comando `pwd` devuelve el directorio de inicio de tu usuario.
Si `pwd` devuelve algo diferente, es posible que necesites navegar allí usando `cd`
o algunos comandos en esta lección no funcionarán como se muestra.
Consulte [Explorando otros directorios](#explorando-otros-directorios) para obtener más detalles
sobre el comando `cd`.


::::::::::::::::::::::::::::::::::::::::::::::::::

Para comprender qué es un 'directorio de inicio',
echemos un vistazo a cómo se organiza el sistema de archivos en su conjunto. Para el
bien de este ejemplo, ilustraremos el sistema de archivos en la computadora de nuestra científica Nelle. Después de esto
ilustración, aprenderás comandos para explorar tu propio sistema de archivos,
que se construirá de manera similar, pero no será exactamente idéntico.

En la computadora de Nelle, el sistema de archivos se ve así:

![](fig/filesystem.svg){alt='El sistema de archivos se compone de un directorio raíz que contiene subdirectorios titulados bin, datos, usuarios y tmp'}

El sistema de archivos se parece a un árbol invertido.
El directorio superior más importante es el **directorio raíz**,
que contiene todo lo demás.
Nos referimos a él usando un carácter de barra diagonal, `/` por sí mismo;
este carácter es la barra diagonal inicial en `/Users/nelle`.

Dentro de ese directorio hay varios otros directorios:
`bin` (donde se almacenan algunos programas incorporados),
`datos` (para archivos de datos diversos),
`usuarios` (donde se encuentran los directorios personales de los usuarios),
`tmp` (para archivos temporales que no necesitan almacenarse a largo plazo),
y así sucesivamente.

Sabemos que nuestro directorio de trabajo actual `/Users/nelle` se almacena dentro de `/Users`
porque `/Users` es la primera parte de su nombre.
De manera similar,
sabemos que `/Users` se almacena dentro del directorio raíz `/`
porque su nombre comienza con `/`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Slashes

Tenga en cuenta que hay dos significados para el carácter `/`.
Cuando aparece al principio de un nombre de archivo o directorio,
se refiere al directorio raíz. Cuando aparece *dentro* de una ruta,
es solo un separador.


::::::::::::::::::::::::::::::::::::::::::::::::::

Debajo de `/Users`,
encontramos un directorio para cada usuario con una cuenta en la máquina de Nelle,
sus colegas *imhotep* y *larry*.

![](fig/home-directories.svg){alt='Al igual que otros directorios, los directorios de inicio son subdirectorios debajo de"/Usuarios" como "/Usuarios/imhotep", "/Usuarios/larry" o "/Usuarios/nelle"'}

Los archivos del usuario *imhotep* se almacenan en `/Users/imhotep`,
los de *larry* en `/Users/larry`,
y los de Nelle en `/Users/nelle`. Nelle es la usuaria en nuestros
ejemplos aquí; por lo tanto, obtenemos `/Usuarios/nelle` como nuestro directorio de inicio.
Normalmente, cuando abres un nuevo símbolo del sistema, estarás en
tu directorio de inicio para comenzar.

Ahora aprendamos el comando que nos permitirá ver el contenido de nuestro
propio sistema de archivos. Podemos ver lo que hay en nuestro directorio de inicio ejecutando `ls`:

```bash
$ ls
```

```output
Aplicaciones Documentos Biblioteca Música Público
Escritorio Descargas Películas Imágenes
```

(Otra vez, tus resultados pueden ser ligeramente diferentes dependiendo de tu sistema operativo y cómo hayas personalizado tu sistema de archivos).

`ls` imprime los nombres de los archivos y directorios en el directorio actual.
Podemos hacer que su salida sea más comprensible usando la opción `-F`
que le dice a `ls` que clasifique la salida
agregando un marcador a los nombres de archivo y directorio para indicar qué son:

- una barra diagonal final indica que se trata de un directorio
- `@` indica un enlace
- `*` indica un ejecutable

Dependiendo de la configuración predeterminada de tu terminal,
la terminal también puede usar colores para indicar si cada entrada es un archivo o un
directorio.

```bash
$ ls -F
```

```output
Aplicaciones/ Documentos/ Biblioteca/ Música/ Público/
Escritorio/ Descargas/ Películas/ Imágenes/
```

Aquí,
podemos ver que el directorio raíz contiene solo **subdirectorios**.
Cualquier nombre en la salida que no tenga un símbolo de clasificación
son **archivos** en el directorio de trabajo actual.

:::::::::::::::::::::::::::::::::::::::::  callout

## Limpiar tu terminal

Si tu pantalla se vuelve demasiado desordenada, puedes limpiar tu terminal usando el
comando `clear`. Aún puedes acceder a comandos anteriores usando <kbd>↑</kbd>
y <kbd>↓</kbd> para moverte línea por línea, o desplazándote en tu terminal.


::::::::::::::::::::::::::::::::::::::::::::::::::

### Obtener ayuda

`ls` tiene muchas otras **opciones**. Hay dos formas comunes de averiguar cómo
usar un comando y qué opciones acepta: -
**dependiendo de tu entorno, es posible que solo una de estas formas funcione:**

1. Podemos pasar la opción `--help` a cualquier comando (disponible en Linux y Git Bash), por ejemplo:

   ```bash
   $ ls --help
   ```

2. Podemos leer el manual con `man` (disponible en Linux y macOS):

   ```bash
   $ man ls
   ```

Describiremos ambas formas a continuación.

:::::::::::::::::::::::::::::::::::::::::  callout

## Ayuda para comandos internos

Algunos comandos están integrados en la terminal Bash, en lugar de existir como programas separados en el sistema de archivos. Un ejemplo es el comando `cd` (cambiar directorio). Si obtienes un mensaje como `No manual entry for cd`, intenta usar `help cd` en su lugar. El comando `help` es como obtienes información de uso para [comandos integrados en Bash](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html).


::::::::::::::::::::::::::::::::::::::::::::::::::

#### La opción `--help`

La mayoría de los comandos de bash y programas que las personas han escrito para ser
ejecutados desde dentro de bash, admiten una opción `--help` que muestra más
información sobre cómo usar el comando o programa.

```bash
$ ls --help
```

```output
Uso: ls [OPCIÓN]... [ARCHIVO]...
Muestra información sobre los ARCHIVOs (el directorio actual de forma predeterminada).
Ordena alfabéticamente si no se especifica -cftuvSUX ni --sort.

  -a, --all                  No ignorar las entradas que comienzan con '.'
  -A, --almost-all           No listar implícitamente '.' y '..'
      --author               con -l, imprimir el autor de cada archivo
  -b, --escape               Imprimir caracteres de nomenclatura de escape de estilo C para caracteres no gráficos
      --block-size=TAMAÑO     escalar tamaños por TAMAÑO antes de imprimirlos; por ejemplo, '--block-size=M' imprime tamaños en unidades de
                               1,048,576 bytes; ver formato de TAMAÑO abajo
  -B, --ignore-backups       No listar entradas respaldadas implícitamente que terminan en ~
  -c                         con -lt: ordenar por y mostrar ctimes (tiempo de cambio de estado de archivo); con -l: mostrar tiempos de cambio de estado de archivo
                               y ordenar por nombre de archivo; de lo contrario ordenar por tiempos de cambio de estado, más nuevos primero
  -C                         listar entradas por columnas
      --color[=CUANDO]        colorear la salida; CUANDO puede ser 'nunca' (valor por omisión si se omite), 'auto' o 'always'; más información a continuación
  -d, --directory            listar solo los nombres de los directorios, no sus contenidos
  -D, --dired                generar la salida diseñada para el modo dired de Emacs
  -f                         no ordenar, activar -aU, desactivar -ls --color
  -F, --classify             añadir indicador (uno de */=>@|) a las entradas
...        ...        ...
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Opciones de la línea de comandos no soportadas

Si intentas usar una opción que no es compatible, `ls` y otros comandos
generalmente mostrarán un mensaje de error similar a:

```bash
$ ls -j
```

```error
ls: opción no válida -- 'j'
Pruébelo con 'ls --help' para más información.
```

::::::::::::::::::::::::::::::::::::::::::::::::::

#### El comando `man`

La otra forma de aprender sobre `ls` es escribir

```bash
$ man ls
```

El comando convertirá tu terminal en una página con una descripción
del comando `ls` y sus opciones.

Para navegar por las páginas `man`,
puedes usar <kbd>↑</kbd> y <kbd>↓</kbd> para moverte línea por línea,
o puedes probar usar <kbd>B</kbd> y <kbd>Barra espaciadora</kbd> para saltar hacia arriba y abajo
por una página completa. Para buscar un carácter o palabra en las páginas `man`,
usa <kbd>/</kbd> seguido del carácter o palabra que estás buscando.
A veces, una búsqueda dará como resultado múltiples resultados.
Si es así, puedes moverte entre los resultados usando <kbd>N</kbd> (para avanzar) y
<kbd>Mayús</kbd>+<kbd>N</kbd> (para retroceder).

Para **salir** de las páginas `man`, presiona <kbd>Q</kbd>.

:::::::::::::::::::::::::::::::::::::::::  callout

## Páginas man en la web

Por supuesto, hay una tercera forma de acceder a ayuda para comandos:
buscar en Internet a través de tu navegador web.
Al utilizar la búsqueda en Internet, incluir la frase `man page de unix` en tu búsqueda
ayudará a encontrar resultados relevantes.

GNU proporciona enlaces a sus
[manuales](https://www.gnu.org/manual/manual.html), incluido el
[núcleo de utilidades GNU](https://www.gnu.org/software/coreutils/manual/coreutils.html),
que cubre muchos de los comandos presentados en esta lección.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Explorando más opciones de `ls`

También puedes usar dos opciones al mismo tiempo. ¿Qué hace el comando `ls` cuando se usa
la opción `-l`? ¿Y qué pasa si usas tanto la opción `-l` como la opción `-h`?

Algunas de sus salidas tratan sobre propiedades que no cubrimos en esta lección (como permisos de archivo y propiedad), pero el resto debería ser útil de todos modos.

:::::::::::::::  solution

## Solución

La opción `-l` permite que `ls` use un formato de lista **l**argo, mostrando no solo
los nombres de archivos / directorios sino también información adicional, como el tamaño del archivo
y la hora de su última modificación. Si usas tanto la opción `-h` como la opción `-l`,
esto hace que el tamaño del archivo sea "legible por personas", es decir, se muestra algo como `5.3K`
en lugar de `5369`.


:::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Listar en orden cronológico inverso

Por defecto, `ls` lista los contenidos de un directorio en orden alfabético
por nombre. El comando `ls -t` lista los elementos por la hora del último
cambio en lugar de por orden alfabético. El comando `ls -r` lista
los contenidos de un directorio en orden inverso.
¿Cuál es el archivo que se muestra al final cuando combinas las opciones `-t` y `-r`?
Pista: Es posible que necesites usar la opción `-l` para ver
las fechas de últimos cambios.

:::::::::::::::  solution

## Solución

El archivo que se encuentra más recientemente modificado se muestra al final cuando se usa `-tr`. Esto
puede ser muy útil para encontrar tus ediciones más recientes o verificar
si se creó un nuevo archivo de salida.


:::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::::::::::::::::

### Explorando otros directorios

No solo podemos usar `ls` en el directorio de trabajo actual,
sino que también podemos usarlo para listar el contenido de otro directorio.
Veamos cómo está el directorio `Escritorio` ejecutando `ls -F Escritorio`,
es decir, el comando `ls` con la opción `-F` y el [**argumento**][Arguments] `Escritorio`.
El argumento `Escritorio` le dice a `ls` que
queremos una lista de algo diferente a nuestro directorio de trabajo actual:

```bash
$ ls -F Escritorio
```

Esto daría como resultado un error si no existe un directorio llamado `Escritorio` en tu directorio de trabajo actual.
Normalmente, un directorio `Escritorio` existe en tu directorio de inicio, que asumimos es tu directorio de trabajo actual de tu terminal.

Tu salida debe ser una lista de todos los archivos y subdirectorios en tu
directorio `Escritorio`, incluido el directorio `shell-lesson-data` que descargaste en la
configuración para esta lección. (En la mayoría de los sistemas, el
contenido del directorio `Escritorio` en la terminal se mostrará como iconos en una interfaz
gráfica de usuario detrás de todas las ventanas abiertas. Mira si este es tu caso).

Organizar las cosas de forma jerárquica nos ayuda a mantener un seguimiento de nuestro trabajo. Si bien es posible poner cientos de archivos en nuestro directorio de inicio de la misma manera que es posible
acumular cientos de hojas impresas en nuestro escritorio, es mucho más fácil encontrar las cosas cuando
se han organizado en subdirectorios con nombres razonables.

Ahora que sabemos que el directorio `shell-lesson-data` se encuentra en nuestro directorio `Escritorio`, podemos hacer dos cosas.

Primero, utilizando la misma estrategia que antes, podemos ver su contenido pasando
un nombre de directorio a `ls`:

```bash
$ ls -F Escritorio/shell-lesson-data
```

Esto muestra el contenido del directorio `/Escritorio/shell-lesson-data`,
porque es donde estamos ahora:

```bash
$ pwd
```

```output
/Users/nelle/Escritorio/shell-lesson-data
```

```bash
$ ls -F
```

```output
ejercicio-datos/  gyre-norte-pacifico/
```

Ahora sabemos que el directorio principal contiene solo **subdirectorios**.
Cualquier nombre en la salida que no tenga un símbolo de clasificación
son **archivos** en el directorio de trabajo actual.

Ahora sabemos cómo bajar por el árbol de directorios (es decir, cómo entrar en un subdirectorio),
¿pero cómo subimos (es decir, cómo salimos de un directorio y entramos a su directorio principal)?
Podríamos intentar lo siguiente:

```bash
$ cd shell-lesson-data
```

Pero obtenemos un error. ¿Por qué?

Con nuestros métodos hasta ahora,
`cd` solo puede ver subdirectorios dentro del directorio actual. Hay
diferentes formas de ver los directorios que están por encima del directorio actual;
empezaremos con el más sencillo.

Hay una forma abreviada en la terminal para volver a un nivel superior. Funciona de la siguiente manera:

```bash
$ cd ..
```

`..` es un nombre especial de directorio que significa
"el directorio que contiene a este",
o más simplemente,
el **directorio principal** del directorio actual.
Si volvemos a ejecutar `pwd` después de ejecutar `cd ..`, volvemos a
"/Users/nelle/Escritorio/shell-lesson-data":

```bash
$ pwd
```

```output
/Users/nelle/Escritorio/shell-lesson-data
```

El directorio especial ".." generalmente no se muestra cuando ejecutamos `ls`. Si queremos mostrarlo, podemos agregar la opción `-a` a `ls -F`:

```bash
$ ls -F -a
```

```output
./  ../  ejercicio-datos/  gyre-norte-pacifico/
```

La opción `-a` significa 'mostrar todos' (incluyendo archivos ocultos);
obliga a `ls` a mostrarnos nombres de archivo y directorio que comienzan con `.`,
como `..` (que, si estamos en `/Users/nelle`, se refiere al directorio `/Users`).
Como puedes ver,
también muestra otro directorio especial que se llama solo `.`,
que significa 'el directorio de trabajo actual'.
Puede parecer redundante tener un nombre para esto,
pero pronto veremos algunos usos para ello.

Toma en cuenta que en la mayoría de las herramientas de línea de comandos, múltiples opciones pueden combinarse
con un solo `-` y sin espacios entre ellas; `ls -F -a` es equivalente a `ls -Fa`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Otros archivos ocultos

Además de los directorios ocultos `..` y `.`, también puedes ver un archivo
llamado `.bash_profile`. Este archivo generalmente contiene la configuración de la terminal.
También puedes ver otros archivos y directorios que comienzan
con `.`. Estos suelen ser archivos y directorios que se utilizan para configurar
diferentes programas en tu computadora. Se utiliza el prefijo `.` para evitar que estos
archivos de configuración se agreguen cuando se usa el comando `ls` estándar.


::::::::::::::::::::::::::::::::::::::::::::::::::

Estos tres comandos son los comandos básicos para navegar por el sistema de archivos en tu computadora:
`pwd`, `ls` y `cd`. Ahora exploremos algunas variaciones de esos comandos. ¿Qué sucede
si escribes `cd` sin dar
un directorio como argumento?

```bash
$ cd
```

¿Cómo puedes verificar lo que sucedió? ¡`pwd` te da la respuesta!

```bash
$ pwd
```

```output
/Users/nelle
```

Resulta que `cd` sin un argumento te devuelve a tu directorio de inicio,
lo cual es genial si te has perdido en tu propio sistema de archivos.

Intentemos volver al directorio `ejercicio-datos` de antes. La última vez, usamos
tres comandos, pero en realidad podemos concatenar la lista de directorios
para ir a `ejercicio-datos` de una vez:

```bash
$ cd ~/Escritorio/shell-lesson-data/ejercicio-datos
```

Verifica que hayamos llegado al lugar correcto ejecutando `pwd` y `ls -F` .

Si queremos subir un nivel desde el directorio de datos, podríamos usar `cd ..`. Pero
hay otra forma de moverse a cualquier directorio, independientemente de tu
ubicación actual.

Hasta ahora, al especificar nombres de directorio, o incluso una ruta de directorio (como se muestra arriba),
hemos estado usando **rutas relativas**. Cuando usas una ruta relativa con un comando
como `ls` o `cd`, intenta encontrar esa ubicación desde donde nos encontremos,
en lugar de desde la raíz del sistema de archivos.

Sin embargo, es posible especificar una **ruta absoluta** a un directorio al
incluir toda su ruta desde el directorio raíz, lo cual se indica con una
barra diagonal inicial. La barra diagonal inicial `/` le dice a la computadora que siga la ruta desde
la raíz del sistema de archivos, por lo que siempre se refiere a un directorio específico,
no importa donde estemos cuando ejecutamos el comando.

Esto nos permite movernos a nuestro directorio `shell-lesson-data` desde cualquier lugar del
sistema de archivos (incluido desde dentro de `ejercicio-datos`). Para encontrar la ruta absoluta
que estamos buscando, podemos usar `pwd` y luego extraer la parte que necesitamos
para movernos a `shell-lesson-data`.

```bash
$ pwd
```

```output
/Users/nelle/Escritorio/shell-lesson-data/ejercicio-datos
```

```bash
$ cd /Users/nelle/Escritorio/shell-lesson-data
```

Ejecuta `pwd` y `ls -F` para asegurarte de que estés en el directorio que esperabas.

:::::::::::::::::::::::::::::::::::::::::  callout

## Otros dos atajos

La terminal interpreta un carácter de tilde (`~`) al comienzo de una ruta como
"el directorio de inicio del usuario actual". Por ejemplo, si el directorio de inicio de Nelle es `/Users/nelle`, entonces `~/datos` es equivalente a
`/Users/nelle/datos`. Esto solo funciona si es el primer carácter en la
ruta; `aquí/allá/~/allá` *no* es lo mismo que `aquí/allá/Users/nelle/allá`.

Otro atajo es el carácter `-`. `cd` traducirá `-` en
*el directorio anterior en el que estuve*, lo cual es más rápido que tener que recordar,
y luego escribir, toda la ruta. Esta es una forma *muy* eficiente de moverse
*adelante y atrás entre dos directorios*; es decir, si ejecutas `cd -` dos veces,
terminarás de nuevo en el directorio de inicio.

La diferencia entre `cd ..` y `cd -` es
que el primero te sube mientras que el último te regresa.

***

¡Inténtalo!
En primer lugar, navega a `~/Escritorio/shell-lesson-data` (que ya deberías estar).

```bash
$ cd ~/Escritorio/shell-lesson-data
```

Luego, ¿`cd` en el directorio `ejercicio-datos/creaturas`

```bash
$ cd ejercicio-datos/creaturas
```

Ahora, si ejecutas

```bash
$ cd -
```

verás que vuelves a `~/Escritorio/shell-lesson-data`.
Ejecuta `cd -` nuevamente y estarás de vuelta en `~/Escritorio/shell-lesson-data/ejercicio-datos/creaturas`


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Rutas absolutas vs relativas

Supongamos que a partir de `/Users/nelle/datos`,
¿Qué comandos podría usar Nelle para navegar a su directorio de inicio,
que es `/Users/nelle`?

1. `cd .`
2. `cd /`
3. `cd /home/nelle`
4. `cd ../..`
5. `cd ~`
6. `cd home`
7. `cd ~/datos/..`
8. `cd`
9. `cd ..`

:::::::::::::::  solution

## Solución

1. No: `.` representa el directorio actual.
2. No: `/` representa el directorio raíz.
3. No: el directorio de inicio de Nelle es `/Users/nelle`.
4. No: este comando sube dos niveles, es decir, termina en `/Users`.
5. Sí: `~` representa el directorio de inicio del usuario, que en este caso es `/Users/nelle`.
6. No: este comando intentaría navegar a un directorio llamado `home` en el directorio actual
  si existe.
7. Sí: innecesariamente complicado, pero correcto.
8. Sí: acceso directo para volver al directorio de inicio del usuario.
9. Sí: sube un nivel.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Resolución de rutas relativas

Usando el diagrama del sistema de archivos a continuación, si `pwd` muestra `/Users/thing`,
¿qué mostrará `ls -F ../backup`?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original/ pnas_final/ pnas_sub/`

![](fig/filesystem-challenge.svg){alt='Un árbol de directorios debajo del directorio de Usuarios donde "/Users" contiene losdirectorios "backup" y "thing"; "/Users/backup" contiene"original","pnas\_final" y "pnas\_sub"; y "/Users/thing" contiene"backup"; y "/Users/thing/backup" contiene "2012-12-01", "2013-01-08"y "2013-01-27"'}

:::::::::::::::  solution

## Solución

1. No: *sí existe* un directorio `backup` en `/Users`.
2. No: este es el contenido de `/Users/thing/backup`,
  pero con `..`, pedimos un nivel superior.
3. No: vea la explicación anterior.
4. Sí: `../backup/` se refiere a `/Users/backup/`.
  
  

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Comprensión de la lectura de `ls`

Usando el diagrama del sistema de archivos a continuación,
si `pwd` muestra `/Users/backup`,
y `-r` indica a `ls` que muestre las cosas en orden inverso,
¿qué comando(s) dará como resultado la siguiente salida?

```output
pnas_sub/ pnas_final/ original/
```

![](fig/filesystem-challenge.svg){alt='Un árbol de directorios debajo del directorio de Usuarios donde "/Users" contiene losdirectorios "backup" y "thing"; "/Users/backup" contiene"original","pnas\_final" y "pnas\_sub"; y "/Users/thing" contiene"backup"; y "/Users/thing/backup" contiene "2012-12-01", "2013-01-08"y "2013-01-27"'}

1. `ls pwd`
2. `ls -r -F`
3. `ls -r -F /Users/backup`

:::::::::::::::  solution

## Solución

1. No: `pwd` no es el nombre de un directorio.
2. Sí: `ls` sin argumento de directorio lista archivos y directorios
  en el directorio actual.
3. Sí: usa la ruta absoluta de manera explícita.
  
  

::::::::::::::::::::::::::::::::::::::::::::::::::

## Sintaxis general de un comando de terminal

Hasta ahora hemos encontrado comandos, opciones y argumentos,
pero tal vez sea útil formalizar algo de terminología.

Considera el siguiente comando como un ejemplo general de un comando,
que descompondremos en sus partes componentes:

```bash
$ ls -F /
```

![](fig/shell_command_syntax.svg){alt='Sintaxis general de un comando de terminal'}

`ls` es el **comando**, con una **opción** `-F` y un
**argumento** `/`.
Ya hemos encontrado opciones que
comienzan con un guión singular (`-`), conocidas como **opciones cortas**,
o dos guiones (`--`), conocidas como **opciones largas**.
[Opciones] cambian el comportamiento de un comando y
[Argumentos] le dicen al comando en qué operar (p. ej., archivos y directorios).
A veces se les llaman opciones y argumentos como **parámetros**.
Un comando puede ser llamado con más de una opción y más de un argumento, pero un
comando no siempre requiere un argumento o una opción.

A veces puedes encontrar que las opciones se denominan **interruptores** o **banderas**,
especialmente para opciones que no requieren ningún argumento. En esta lección nos ceñiremos
al uso del término *opción*.

Cada parte está separada por espacios. Si omites el espacio
entre `ls` y `-F`, la terminal buscará un comando llamado `ls-F`, que
no existe. Además, la capitalización puede ser importante.
Por ejemplo, `ls -s` mostrará el tamaño de los archivos y directorios junto a los nombres,
mientras que `ls -S` ordenará los archivos y directorios por tamaño, como se muestra a continuación:

```bash
$ cd ~/Escritorio/shell-lesson-data
$ ls -s ejercicio-datos
```

```output
total 28
 4 animal-counts   4 creatures  12 numbers.txt   4 alkanes   4 writing
```

Toma en cuenta que los tamaños que devuelve `ls -s` están en *bloques*.
Como están definidos de manera diferente para diferentes sistemas operativos,
es posible que no obtengas las mismas cifras que se muestran en el ejemplo.

```bash
$ ls -S ejercicio-datos
```

```output
animal-counts  creatures  alkanes  writing  numbers.txt
```

Poniendo todo junto, nuestro comando `ls -F /` anterior nos da una lista
de archivos y directorios en el directorio raíz `/`.
Un ejemplo de la salida que podrías obtener del comando anterior se muestra a continuación:

```bash
$ ls -F /
```

```output
Aplicaciones/         Sistema/
Biblioteca/              Usuarios/
Red/              Volúmenes/
```


:::::::::::::::::::::::::::::::::::::::: keypoints

- El sistema de archivos es responsable de administrar la información en el disco.
- La información se almacena en archivos, que se almacenan en directorios (carpetas).
- Los directorios también pueden almacenar otros directorios, que luego forman un árbol de directorios.
- `pwd` imprime el directorio de trabajo actual del usuario.
- `ls [ruta]` muestra una lista específica de archivos o directorios; `ls` solo muestra el directorio de trabajo actual.
- `cd [ruta]` cambia el directorio de trabajo actual.
- La mayoría de los comandos toman opciones que comienzan con un solo guión.
- Los nombres de directorio en la ruta se separan con `/` en Unix, pero con `\` en Windows.
- `/` por sí solo es el directorio raíz del sistema de archivos completo.
- Una ruta absoluta especifica una ubicación desde la raíz del sistema de archivos.
- Una ruta relativa especifica una ubicación a partir de la ubicación actual.
- `.` por sí solo significa 'el directorio actual'; `..` significa 'el directorio superior al actual'.

::::::::::::::::::::::::::::::::::::::::::::::::::