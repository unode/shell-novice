---
title: Navegando por archivos y directorios
teaching: 30
exercises: 10
---

::::::::::::::::::::::::::::::::::::::: objectives

- Explicar las similitudes y diferencias entre un archivo y un directorio.
- Traducir una ruta absoluta en una ruta relativa y viceversa.
- Construir rutas absolutas y relativas que identifiquen archivos y directorios específicos.
- Usar opciones y argumentos para cambiar el comportamiento de un comando de la terminal.
- Demostrar el uso de completado de pestañas y explicar sus ventajas.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo moverme en mi computadora?
- ¿Cómo puedo ver qué archivos y directorios tengo?
- ¿Cómo puedo especificar la ubicación de un archivo o directorio en mi computadora?

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: instructor

Introducirse y navegar por el sistema de archivos en la terminal
(cubierto en la sección [Navigating Files and Directories](02-filedir.md))
puede ser confuso. Puede tener la terminal y el explorador de archivos
gráfico abiertos lado a lado para que los estudiantes puedan ver el contenido y la estructura
de directorios mientras usan la terminal para navegar por el sistema.

::::::::::::::::::::::::::::::::::::::::::::::::::

La parte del sistema operativo responsable de administrar archivos y directorios
es llamada el **sistema de archivos**.
Organiza nuestros datos en archivos,
que contienen información,
y directorios (también llamados 'carpetas'),
que contienen archivos u otros directorios.

Se utilizan varios comandos para crear, inspeccionar, renombrar y eliminar archivos y directorios.
Para comenzar a explorarlos, vamos a abrir la ventana de la terminal.

Primero, veamos dónde estamos ejecutando el comando `pwd`
(que significa 'directorio de impresión de trabajo'). Los directorios son como *lugares*: en cualquier momento
mientras usamos la terminal, estamos en exactamente un lugar llamado
nuestro **directorio de trabajo actual**. Los comandos leen y escriben principalmente archivos en el
directorio de trabajo actual, es decir, 'aquí', por lo que es importante saber dónde se encuentra antes de ejecutar
un comando. `pwd` muestra dónde estás:

```bash
$ pwd
```

```output
/Users/nelle
```

Aquí, la respuesta de la computadora es `/Users/nelle`,
que es el **directorio de inicio** de Nelle:

:::::::::::::::::::::::::::::::::::::::::  callout

## Variación del directorio de inicio

La ruta del directorio de inicio se verá diferente en diferentes sistemas operativos.
En Linux, puede verse como `/home/nelle`,
y en Windows, será similar a `C:\Documents and Settings\nelle` o
`C:\Users\nelle`.
(Tenga en cuenta que puede verse ligeramente diferente para diferentes versiones de Windows).
En ejemplos futuros, hemos utilizado la salida de Mac como la predeterminada: Linux y Windows
la salida puede diferir ligeramente, pero generalmente debería ser similar.

También asumiremos que su comando `pwd` devuelve el directorio de inicio de su usuario actual.
Si`pwd` devuelve algo diferente, es posible que deba navegar allí usando `cd`
o algunos comandos en esta lección no funcionarán como se escribió.
Consulte [Exploración de otros directorios](#exploración-de-otros-directorios) para más detalles
sobre el comando `cd`.


::::::::::::::::::::::::::::::::::::::::::::::::::

Para comprender qué es un 'directorio de inicio',
veamos cómo se organiza el sistema de archivos en su conjunto. Para el
ejemplo, estaremos ilustrando el sistema de archivos en la computadora del científico Nelle. Después de este
ilustración, aprenderá comandos para explorar su propio sistema de archivos,
que se construirá de manera similar, pero no será exactamente idéntico.

En la computadora de Nelle, el sistema de archivos se ve así:

![](fig/filesystem.svg){alt='El sistema de archivos se compone de un directorio raíz que contiene subdirectorios llamados bin, datos, usuarios y tmp'}

El sistema de archivos se parece a un árbol al revés.
El directorio superior es el **directorio raíz**,
que contiene todo el resto.
Nos referimos a él usando un carácter de barra inclinada `/`,
por sí solo;
Este carácter es la barra inclinada inicial en `/Users/nelle`.

Dentro de ese directorio hay varios otros directorios:
`bin` (donde se almacenan algunos programas integrados),
`datos` (para archivos de datos diversos),
`Usuarios` (donde se encuentran los directorios personales de los usuarios),
`tmp` (para archivos temporales que no necesitan almacenarse a largo plazo),
y así sucesivamente.

Sabemos que nuestro directorio de trabajo actual `/Users/nelle` se encuentra dentro de `/Usuarios`
porque `/Usuarios` es la primera parte de su nombre.
Del mismo modo,
sabemos que `/Usuarios` se almacena dentro del directorio raíz `/`
porque su nombre comienza con `/`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Barras

Observe que hay dos significados para el carácter `/`.
Cuando aparece al principio de un archivo o nombre de directorio,
se refiere al directorio raíz. Cuando aparece *dentro* de una ruta,
es solo un separador.


::::::::::::::::::::::::::::::::::::::::::::::::::

Debajo de `/Usuarios`,
encontramos un directorio para cada usuario con una cuenta en la máquina de Nelle,
sus colegas *imhotep* y *larry*.

![](fig/home-directories.svg){alt='Al igual que otros directorios, los directorios de inicio son subdirectorios debajo de"/Usuarios" como "/Usuarios/imhotep", "/Usuarios/larry", o"/Usuarios/nelle"'}

Los archivos del usuario *imhotep* se almacenan en `/Usuarios/imhotep`,
los de *larry* en `/Usuarios/larry`,
y los de Nelle en `/Usuarios/nelle`. Nelle es la usuario en nuestros
ejemplos aquí; por lo tanto, obtenemos `/Usuarios/nelle` como nuestro directorio de inicio.
Típicamente, cuando abre un nuevo comando, estará en
su directorio de inicio para comenzar.

Ahora aprendamos el comando que nos permitirá ver el contenido de nuestro
sistema de archivos propio. Podemos ver qué hay en nuestro directorio de inicio ejecutando `ls`:

```bash
$ ls
```

```output
Aplicaciones Documentos Biblioteca Música Público
Escritorio    Descargas    Películas    Imágenes
```

(Nuevamente, sus resultados pueden ser ligeramente diferentes dependiendo de su sistema operativo
y cómo haya personalizado su sistema de archivos).

`ls` muestra los nombres de los archivos y directorios en el directorio actual.
Podemos hacer que su salida sea más comprensible usando la **opción** `-F`
que indica a `ls` que clasifique la salida
agregando un indicador a los nombres de archivo y directorio para indicar qué son:

- una barra inclinada `/` al final indica que este es un directorio
- `@` indica un enlace
- `*` indica un ejecutable

Dependiendo de la configuración predeterminada de su terminal,
el shell también puede usar colores para indicar si cada entrada es un archivo o
directorio.

```bash
$ ls -F
```

```output
Aplicaciones/ Documentos/ Biblioteca/ Música/ Público/
Escritorio/    Descargas/    Películas/    Imágenes/
```

Aquí,
podemos ver que el directorio de inicio contiene solo **subdirectorios**.
Cualquier nombre en la salida que no tenga un símbolo de clasificación
son **archivos** en el directorio de trabajo actual.

:::::::::::::::::::::::::::::::::::::::::  callout

## Limpiar su terminal

Si su pantalla se llena de información, puede limpiar su terminal usando el
comando `clear`. Aún puede acceder a comandos anteriores utilizando <kbd>↑</kbd>
y <kbd>↓</kbd> para moverse por línea o desplazarse hacia arriba y abajo en su terminal.


::::::::::::::::::::::::::::::::::::::::::::::::::

### Obtención de ayuda

`ls` tiene muchas otras **opciones**. Hay dos formas comunes de averiguar cómo
usar un comando y qué opciones acepta:
dependiendo de su entorno, puede encontrar que solo una de estas opciones funciona:

1. Podemos pasar una opción `--help` a cualquier comando (disponible en Linux y Git Bash), por ejemplo:

  ```bash
  $ ls --help
  ```

2. Podemos leer su manual con `man` (disponible en Linux y macOS):

  ```bash
  $ man ls
  ```

Describiremos ambas formas a continuación.

:::::::::::::::::::::::::::::::::::::::::  callout

## Ayuda para comandos integrados

Algunos comandos están integrados en la shell Bash, en lugar de existir como programas separados
en el sistema de archivos. Un ejemplo es el comando `cd` (cambiar directorio).
Si obtiene un mensaje como `No se encontró ninguna entrada para cd`, intente `help cd` en su lugar. El
comando `help` es cómo se obtiene información de uso para los
Built-ins de Bash
 [built-ins](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html).


::::::::::::::::::::::::::::::::::::::::::::::::::

#### La opción `--help`

La mayoría de los comandos de bash y programas que las personas han escrito para ser
ejecutados desde dentro de bash admiten una opción `--help` que muestra más
información sobre cómo usar el comando o programa.

```bash
$ ls --help
```

```output
Uso: ls [OPCIÓN]... [ARCHIVO]...
Listar información sobre los ARCHIVOs (por defecto, el directorio actual).
Ordena las entradas alfabéticamente si no se especifica -cftuvSUX ni --sort.

Los argumentos obligatorios de las opciones largas son obligatorios para las opciones cortas, también.
  -a, --all                  no ocultar las entradas que comienzan con .
  -A, --almost-all           ocultar las entradas implicadas . y ..
      --author               con -l, imprimir el autor de cada archivo
  -b, --escape               imprimir escapes de estilo C para caracteres no gráficos
      --block-size=TAMAÑO    escalar los tamaños por TAMAÑO antes de imprimirlos; p.e., '--block-size=M' imprime tamaños en unidades de
                               1,048,576 bytes; ver el formato TAMAÑO más abajo
  -B, --ignore-backups       no listar las entradas implícitas que terminan con ~
  -c                         con -lt: ordenar por, y mostrar, ctime (hora de la última modificación del estado del archivo);
                               con -l: mostrar ctime y ordenar por nombre;
                               en otro caso: ordenar por ctime, más reciente primero
  -C                         listar las entradas de varios en columnas
      --color[=CUÁNDO]       colorear la salida; CUÁNDO puede ser 'always' (si se omite, por defecto),
                               'auto' o 'never'; más información más abajo
  -d, --directory            mostrar solamente los directorios en sí, no sus contenidos
  -D, --dired                generar una salida diseñada para el modo de edición de dired de Emacs
  -f                         no ordenar, activar -aU, desactivar -ls --color
  -F, --classify             añadir un carácter a las entradas (-, /, COM, =, *, @, |)
...        ...        ...
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Opciones de línea de comandos no admitidas

Si intenta usar una opción que no está admitida, `ls` y otros comandos
suelen mostrar un mensaje de error similar a este:

```bash
$ ls -j
```

```error
ls: opción no válida -- 'j'
Intente 'ls --help' para obtener más información.
```

::::::::::::::::::::::::::::::::::::::::::::::::::

#### El comando `man`

La otra forma de aprender acerca del comando `ls` es escribir

```bash
$ man ls
```

Este comando convertirá su terminal en una página con la descripción
del comando `ls` y sus opciones.

Para navegar por las páginas `man`,
puede usar <kbd>↑</kbd> y <kbd>↓</kbd> para moverse línea por línea,
o <kbd>B</kbd> y <kbd>Spacebar</kbd> para desplazarse hacia arriba y hacia abajo por una página completa.
Para buscar un carácter o palabra en las páginas `man`,
use <kbd>/</kbd> seguido del carácter o palabra que está buscando.
A veces, una búsqueda dará como resultado múltiples coincidencias.
Si es así, puede moverse entre ellas usando <kbd>N</kbd> (para avanzar) y
<kbd>Shift</kbd>\+<kbd>N</kbd> (para retroceder).

Para **salir** de las páginas `man`, presione <kbd>Q</kbd>.

:::::::::::::::::::::::::::::::::::::::::  callout

## Páginas de manual en la web

Por supuesto, existe una tercera forma de acceder a la ayuda para los comandos:
buscar en internet a través de su navegador web.
Cuando use la búsqueda en Internet, incluir la frase `página de manual de Unix` en su consulta será útil para encontrar resultados relevantes.

GNU proporciona enlaces a sus
[manuales](https://www.gnu.org/manual/manual.html), que incluyen los
[core GNU utilities](https://www.gnu.org/software/coreutils/manual/coreutils.html),
que cubre muchos comandos introducidos en esta lección.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Explorando más opciones de `ls`

También puede usar dos opciones al mismo tiempo. ¿Qué hace el comando `ls` cuando se usa
con la opción `-l`? ¿Qué pasa si se usan tanto la opción `-l` como la opción `-h`?

Parte de su salida está relacionada con propiedades que no cubrimos en esta lección (como
los permisos de archivo y la propiedad), pero el resto debería ser útil
de todos modos.

:::::::::::::::  solution

## Solución

La opción `-l` hace que `ls` use un formato de lista **l**(Tipo), mostrando no solo
los nombres de archivo/directorio sino también información adicional, como el tamaño del archivo
y la hora de su última modificación. Si usa tanto la opción `-h` como la opción `-l`,
esto hace que el tamaño del archivo sea '**h**umano legible', es decir, que se muestre algo como `5.3K`
en lugar de `5369`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Orden de lista en orden cronológico inverso

De forma predeterminada, `ls` lista el contenido de un directorio en orden alfabético
por nombre. El comando `ls -t` lista los elementos por hora de último
cambio en lugar de alfabéticamente. El comando `ls -r` lista el
contenido de un directorio en orden inverso.
¿Qué archivo se muestra al final cuando combina las opciones `-t` y `-r`?
Pista: Es posible que deba usar la opción `-l` para ver las
fechas de cambio más recientes.

:::::::::::::::  solution

## Solución

El archivo que cambió más recientemente se muestra al final al usar `-rt`. Esto
puede ser muy útil para encontrar sus ediciones más recientes o verificar
si se escribió un nuevo archivo de salida.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

### Exploración de otros directorios

No solo podemos usar `ls` en el directorio de trabajo actual,
sino que también podemos usarlo para enumerar el contenido de un directorio diferente.
Veamos nuestro directorio `Escritorio` ejecutando `ls -F Escritorio`,
es decir,
el comando `ls` con la opción `-F` y el [**argumento**][Arguments]  `Escritorio`.
El argumento `Escritorio` le dice a `ls` que
queremos una lista de algo que no sea nuestro directorio de trabajo actual:

```bash
$ ls -F Escritorio
```

Esto devolverá un error si no existe un directorio llamado `Escritorio` en su directorio
de trabajo actual. Típicamente, hay un directorio llamado `Escritorio` en su
directorio de inicio, que asumimos que es el directorio de trabajo actual de su `bash` shell.

Su salida debería ser una lista de todos los archivos y subdirectorios en su
directorio de escritorio, incluido el directorio `datos-leccion-terminal` que descargará en la
[configuración de esta lección](../learners/setup.md). (En la mayoría de los sistemas, el
contenido del directorio `Escritorio` en la terminal se mostrará como iconos en una interfaz
gráfica de usuario detrás de todas las ventanas abiertas. Vea si esto se aplica a su caso).

Organizar las cosas jerárquicamente nos ayuda a mantener un registro de nuestro trabajo. Aunque es
posible colocar cientos de archivos en el directorio de inicio, al igual que es posible
apilar cientos de papeles impresos en un escritorio, es mucho más fácil encontrar cosas cuando
se han organizado en subdirectorios con nombres lógicos.

Ahora que sabemos que el directorio `datos-leccion-terminal` se encuentra en nuestro directorio de escritorio, podemos hacer dos cosas.

Primero, usando la misma estrategia que antes, podemos ver su contenido pasando
un nombre de directorio a `ls`:

```bash
$ ls -F Escritorio/datos-leccion-terminal
```

Segundo, podemos cambiar realmente nuestra ubicación a un directorio diferente, para que ya no
estemos ubicados en
nuestro directorio de inicio.

El comando para cambiar las ubicaciones es `cd` seguido de un
nombre de directorio para cambiar nuestro directorio de trabajo.
`cd` significa 'cambiar directorio',
que es un poco engañoso.
El comando no cambia el directorio;
cambia el directorio actual de trabajo de la shell.
En otras palabras, cambia la configuración de la shell para saber en qué directorio nos encontramos.
El comando `cd` se parece a hacer doble clic en una carpeta en una interfaz gráfica
para entrar en esa carpeta.

Digamos que queremos pasar al directorio `datos-leccion-terminal` que vimos anteriormente. Podemos
usar la siguiente serie de comandos para hacer eso:

```bash
$ cd Escritorio
$ cd datos-leccion-terminal
$ cd datos-ejercicio
```

Estos comandos nos moverán desde nuestro directorio de inicio a nuestro directorio de escritorio, luego a
al directorio `datos-leccion-terminal` y finalmente a le directorio `datos-ejercicio`.
Te darás cuenta de que `cd` no imprime nada. Esto es normal.
Muchos comandos de la shell no emiten ninguna salida en la pantalla cuando se ejecutan correctamente.
Pero si ejecutamos `pwd` después, podemos ver que ahora
estamos en `/Users/nelle/Desktop/datos-leccion-terminal/datos-ejercicio`.

Si ejecutamos `ls -F` sin argumentos ahora,
enumerará el contenido de `/Users/nelle/Desktop/datos-leccion-terminal/datos-ejercicio`,
porque ahí es donde estamos ahora:

```bash
$ pwd
```

```output
/Users/nelle/Desktop/datos-leccion-terminal/datos-ejercicio
```

```bash
$ ls -F
```

```output
alkanes/  animal-counts/  creatures/  numbers.txt  writing/
```

Ahora sabemos cómo bajar por el árbol de directorios (es decir, cómo ingresar a un subdirectorio),
pero ¿cómo subimos (es decir, cómo salimos de un directorio y entramos en su directorio padre)?
Podemos intentar esto:

```bash
$ cd datos-leccion-terminal
```

```error
-bash: cd: datos-leccion-terminal: No such file or directory
```

¡Pero obtenemos un error! ¿Por qué pasa esto?

Con los métodos que hemos visto hasta ahora,
`cd` solo puede ver subdirectorios dentro de su directorio actual. Hay
diferentes formas de ver los directorios por encima de su ubicación actual; comenzaremos
con la más simple.

Hay un atajo en la shell para moverse un nivel hacia arriba en la jerarquía de directorios. Funciona de la siguiente manera:

```bash
$ cd ..
```

`..` es un nombre de directorio especial que significa
"el directorio que contiene este",
o más brevemente,
el **padre** del directorio actual.
Por supuesto,
si ejecutamos `pwd` después de ejecutar `cd ..`, volveremos a `/Users/nelle/Desktop/datos-leccion-terminal`:

```bash
$ pwd
```

```output
/Users/nelle/Desktop/datos-leccion-terminal
```

El directorio especial `..` no suele aparecer cuando ejecutamos `ls`. Si queremos
mostrarlo, podemos agregar la opción `-a` a `ls -F`:

```bash
$ ls -F -a
```

```output
./  ../  ejercicio-datos/  gyre/  goodiff.sh  goostats.sh
```

El directorio `.`, que significa "el directorio de trabajo actual", tampoco suele mostrarse.
Aunque puede parecer redundante tener un nombre para él,
pronto veremos algunos usos para él.

Tenga en cuenta que en la mayoría de las herramientas de línea de comandos, varias opciones se pueden combinar
con un solo `-` y sin espacios entre ellas; `ls -F -a` es
equivalente a `ls -Fa`.

:::::::::::::::::::::::::::::::::::::::::  callout

## Otros archivos ocultos

Además de los directorios ocultos `..` y `.`, también puede ver un archivo
llamado `.bash_profile`. Este archivo generalmente contiene configuración de shell
ajustes. También puede ver otros archivos y directorios que comienzan
con `.`. Estos suelen ser archivos y directorios que se utilizan para configurar
diferentes programas en su computadora. El prefijo `.` se utiliza para evitar que estos
archivos de configuración se amontonen en la terminal cuando se usa un comando
`ls` estándar.


::::::::::::::::::::::::::::::::::::::::::::::::::

Estos tres comandos son los comandos básicos para navegar por el sistema de archivos en su computadora:
`pwd`, `ls` y `cd`. Veamos algunas variaciones de esos comandos. ¿Qué sucede
si escribe `cd` por sí solo, sin especificar
un directorio?

```bash
$ cd
```

¿Cómo puedes verificar qué sucedió? ¡`pwd` nos da la respuesta!

```bash
$ pwd
```

```output
/Users/nelle
```

Resulta que `cd` sin ningún argumento nos devuelve a nuestro directorio de inicio,
que es excelente si nos hemos perdido en nuestro propio sistema de archivos.

Intentemos volver al directorio `ejercicio-datos` de antes. La última vez, usamos
tres comandos, pero en realidad podemos escribir juntos todos los directorios
para moverse a `ejercicio-datos` en un solo paso:

```bash
$ cd Desktop/datos-leccion-terminal/ejercicio-datos
```

Comprueba que te has movido al lugar correcto ejecutando `pwd` y `ls -F`.

Si queremos mover hacia arriba un nivel desde el directorio de datos, podríamos usar `cd ..`. Pero hay
otra forma de moverse a cualquier directorio, independientemente de su
ubicación actual.

Hasta ahora, al especificar nombres de directorio, o incluso una ruta de directorio (como arriba),
hemos estado usando **rutas relativas**. Cuando usas una ruta relativa con un comando
como `ls` o` cd`, intenta encontrar esa ubicación desde donde estamos,
en lugar de desde la raíz del sistema de archivos.

Sin embargo, es posible especificar la **ruta absoluta** a un directorio al
incluir su ruta completa desde el directorio raíz, que se indica con una
barra inclinada inicial. La barra inclinada inicial `/` le dice a la computadora que siga la ruta desde
la raíz del sistema de archivos, por lo que siempre se refiere a exactamente un directorio,
no importa dónde estemos cuando ejecutemos el comando.

Esto nos permite pasar a nuestro directorio de `datos-leccion-terminal` desde cualquier lugar en
el sistema de archivos (incluido desde dentro de` ejercicio-datos`). Para encontrar la ruta absoluta
que estamos buscando, podemos usar `pwd` y luego extraer la parte que necesitamos
para pasar al directorio de `datos-leccion-terminal`.

```bash
$ pwd
```

```output
/Users/nelle/Desktop/datos-leccion-terminal/ejercicio-datos
```

```bash
$ cd /Users/nelle/Desktop/datos-leccion-terminal
```

Ejecuta `pwd` y `ls -F` para asegurarte de que estás en el directorio que esperas.

:::::::::::::::::::::::::::::::::::::::::  callout

## Dos atajos más

La shell interpreta un carácter de tilde (`~`) al comienzo de una ruta como
"el directorio de inicio del usuario actual". Por ejemplo, si el directorio de inicio de Nelle es `/Users/nelle`, entonces `~/datos` es equivalente a `/Users/nelle/datos`. Esto solo ocurre si es el primer carácter de la ruta; `aquí/allá/~` no es `/aquí/allá/Users/nelle/~`.

Otro acceso directo es el carácter `-` (guion). `cd` convertirá `-` en
*el directorio anterior en el que estaba*, lo que es más rápido que tener que recordar,
y luego escribir, la ruta completa. Esta es una forma *muy* eficiente de moverse
*de un directorio a otro* -- es decir, si ejecutas `cd -` dos veces,
terminarás de nuevo en el directorio de inicio.

La diferencia entre `cd ..` y `cd -` es que el primero te lleva *más arriba*, mientras que el segundo te lleva *de vuelta*.

***

¡Inténtalo!
Primero ve a `~/Desktop/datos-leccion-terminal` (ya deberías estar allí).

```bash
$ cd ~/Desktop/datos-leccion-terminal
```

Luego, `cd` al directorio `ejercicio-datos/creatures`

```bash
$ cd ejercicio-datos/creatures
```

Ahora, si ejecutas

```bash
$ cd -
```

verás que estás de vuelta en `~/Desktop/datos-leccion-terminal`.
Ejecuta `cd -` nuevamente y estarás de vuelta en `~/Desktop/datos-leccion-terminal/ejercicio-datos/creatures`


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Rutas absolutas vs. Rutas relativas

A partir de `/Users/nelle/data`,
¿cuál de los siguientes comandos podría usar Nelle para navegar a su directorio de inicio,
que es `/Users/nelle`?

1. `cd .`
2. `cd /`
3. `cd /home/nelle`
4. `cd ../..`
5. `cd ~`
6. `cd home`
7. `cd ~/data/..`
8. `cd`
9. `cd ..`

:::::::::::::::  solution

## Solución

1. No: `.` significa el directorio actual.
2. No: `/` significa el directorio raíz.
3. No: el directorio de inicio de Nelle es `/Users/nelle`.
4. No: este comando sube dos niveles, es decir, termina en `/Users`.
5. Sí: `~` es el directorio de inicio del usuario, en este caso `/Users/nelle`.
6. No: este comando navegaria a un directorio `home` en el directorio actual
   si existe.
7. Sí: innecesariamente complicado, pero correcto.
8. Sí: atajo para volver al directorio de inicio del usuario.
9. Sí: sube un nivel.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Resolución de ruta relativa

Usando el diagrama del sistema de archivos a continuación, si `pwd` muestra `/Users/thing`,
¿qué mostrará `ls -F ../backup`?

1. `../backup: No such file or directory`
2. `2012-12-01 2013-01-08 2013-01-27`
3. `2012-12-01/ 2013-01-08/ 2013-01-27/`
4. `original/ pnas_final/ pnas_sub/`

![](fig/filesystem-challenge.svg){alt='Un árbol de directorios debajo del directorio Usuarios, donde "/Usuarios" contiene losdirectorios "backup" y "thing"; "/Usuarios/backup" contiene "original","pnas\_final" y "pnas\_sub"; "/Usuarios/thing" contiene "backup"; y"/Usuarios/thing/backup" contiene "2012-12-01", "2013-01-08" y"2013-01-27"'}

:::::::::::::::  solution

## Solución

1. No: hay un directorio `backup` en `/Usuarios`.
2. No: esto es el contenido de `Usuarios/thing/backup`,
   pero con `..` les pedimos un nivel más arriba.
3. No: ver la explicación anterior.
4. Sí: `../backup/` se refiere a `/Usuarios/backup/`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Comprensión de lectura de `ls`

Usando el diagrama del sistema de archivos a continuación,
si `pwd` muestra `/Users/backup`,
y `-r` le dice a `ls` que muestre las cosas en orden inverso,
¿qué comando(s) dará como resultado la siguiente salida?

```output
pnas_sub/ pnas_final/ original/
```

![](fig/filesystem-challenge.svg){alt='Un árbol de directorios debajo del directorio Usuarios, donde "/Users" contiene losdirectorios "backup" y "thing"; "/Users/backup" contiene "original","pnas\_final" y "pnas\_sub"; "/Users/thing" contiene "backup"; y"/Users/thing/backup" contiene "2012-12-01", "2013-01-08" y"2013-01-27"'}

1. `ls pwd`
2. `ls -r -F`
3. `ls -r -F /Users/backup`

:::::::::::::::  solution

## Solución

1. No: `pwd` no es el nombre de un directorio.
2. Sí: `ls` sin argumento de directorio enumera archivos y directorios
   en el directorio actual.
3. Sí: usa la ruta absoluta explícitamente.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Sintaxis general de un comando de terminal

Hemos encontrado comandos, opciones y argumentos,
pero tal vez sea útil formalizar algunos conceptos.

Considere el siguiente comando como un ejemplo general de un comando,
que desglosaremos en sus componentes:

```bash
$ ls -F /
```

![](fig/shell_command_syntax.svg){alt='Sintaxis general de un comando de terminal'}

`ls` es el **comando**, con una **opción** `-F` y un
**argumento** `/`.
Ya hemos encontrado opciones que
comienzan con un guion (`-`), conocidas como **opciones cortas**,
o dos guiones (`--`), conocidas como **opciones largas**.
[Las opciones] cambian el comportamiento de un comando y
[los argumentos] indican al comando en qué operar (por ejemplo, archivos y directorios).
A veces, las opciones y los argumentos se refieren como **parámetros**.
Un comando se puede llamar con más de una opción y más de un argumento, pero un
comando no siempre requiere un argumento u opción.

A veces puedes ver que se hace referencia a las opciones como **interruptores** o **banderas**,
especialmente para opciones que no tienen argumentos. En esta lección nos ceñiremos al
uso del término *opción*.

Cada parte está separada por espacios. Si omite el espacio
entre `ls` y `-F` la shell buscará un comando llamado `ls-F`, que
no existe. Además, la capitalización puede ser importante.
Por ejemplo, `ls -s` mostrará el tamaño de los archivos y directorios junto a los nombres,
mientras que `ls -S` ordenará los archivos y directorios por tamaño, como se muestra a continuación:

```bash
$ cd ~/Desktop/datos-leccion-terminal
$ ls -s ejercicio-datos
```

```output
total 28
 4 animal-counts   4 creatures  12 numbers.txt   4 alkanes   4 writing
```

Tenga en cuenta que los tamaños devueltos por `ls -s` están en *bloques*.
Como estos se definen de manera diferente para diferentes sistemas operativos,
puede que no obtenga las mismas cifras que en el ejemplo.

```bash
$ ls -S ejercicio-datos
```

```output
animal-counts  creatures  alkanes  writing  numbers.txt
```

Poniéndolo todo junto, nuestro comando `ls -F /` anterior nos da una lista
de archivos y directorios en el directorio raíz `/`.
Un ejemplo de la salida que podrías obtener del comando anterior se muestra a continuación:

```bash
$ ls -F /
```

```output
Applications/         System/
Library/              Users/
Network/              Volumes/
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- El sistema de archivos se encarga de administrar la información en el disco.
- La información se almacena en archivos, que se almacenan en directorios (carpetas).
- Los directorios también pueden contener otros directorios, formando así un árbol de directorios.
- `pwd` muestra el directorio de trabajo actual del usuario.
- `ls [ruta]` muestra una lista de un archivo o directorio específico; `ls` por sí solo lista el directorio de trabajo actual.
- `cd [ruta]` cambia el directorio de trabajo actual.
- La mayoría de los comandos toman opciones que empiezan con un solo guion `-`.
- Los nombres de directorio en una ruta se separan con `/` en Unix, pero con `\` en Windows.
- `/` por sí solo es el directorio raíz de todo el sistema de archivos.
- Una ruta absoluta especifica una ubicación desde la raíz del sistema de archivos.
- Una ruta relativa especifica una ubicación comenzando desde la ubicación actual.
- `.` por sí solo significa 'el directorio actual'; `..`, 'el directorio anterior al actual'.

::::::::::::::::::::::::::::::::::::::::::::::::::