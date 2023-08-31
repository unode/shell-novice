---
title: Trabajando con archivos y directorios
teaching: 30
exercises: 20
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Crear una jerarquía de directorios que coincida con un diagrama dado.
- Crear archivos en esa jerarquía utilizando un editor o copiando y renombrando archivos existentes.
- Eliminar, copiar y mover archivos y/o directorios especificados.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Cómo puedo crear, copiar y eliminar archivos y directorios?
- ¿Cómo puedo editar archivos?

::::::::::::::::::::::::::::::::::::::::::::::::::


## Creando directorios

Ahora sabemos cómo explorar archivos y directorios,
¿pero cómo los creamos en primer lugar?

En este episodio aprenderemos a crear y mover archivos y directorios,
usando el directorio `exercise-data/writing` como ejemplo.

### Paso uno: ver dónde estamos y qué tenemos

Deberíamos seguir estando en el directorio `shell-lesson-data` en el escritorio,
que podemos verificar usando:

```bash
$ pwd
```

```output
/Users/nelle/Desktop/shell-lesson-data
```

A continuación, nos moveremos al directorio `exercise-data/writing` y veremos qué contiene:

```bash
$ cd exercise-data/writing/
$ ls -F
```

```output
haiku.txt  LittleWomen.txt
```

### Crear un directorio

Creemos un nuevo directorio llamado `thesis` usando el comando `mkdir thesis`
(que no tiene salida):

```bash
$ mkdir thesis
```

Como podrías adivinar por su nombre,
`mkdir` significa 'crear directorio'.
Dado que `thesis` es una ruta relativa
(es decir, no tiene una barra inclinada inicial, como `/what/ever/thesis/`),
el nuevo directorio se crea en el directorio de trabajo actual:

```bash
$ ls -F
```

```output
haiku.txt  LittleWomen.txt  thesis/
```

Dado que acabamos de crear el directorio `thesis`, aún no hay nada en él:

```bash
$ ls -F thesis
```

Ten en cuenta que `mkdir` no se limita a crear directorios individuales de uno en uno.
La opción `-p` permite que `mkdir` cree un directorio con subdirectorios anidados
en una sola operación:

```bash
$ mkdir -p ../project/data ../project/results
```

La opción `-R` en el comando `ls` mostrará todos los subdirectorios anidados dentro de un directorio.
Usemos `ls -FR` para listar de forma recursiva la nueva jerarquía de directorios que acabamos de crear en el
directorio de `project`:

```bash
$ ls -FR ../project
```

```output
../project/:
data/  results/

../project/data:

../project/results:
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Dos formas de hacer lo mismo

El uso de la línea de comandos para crear un directorio no es diferente de usar un explorador de archivos.
Si abres el directorio actual utilizando el explorador de archivos gráfico de tu sistema operativo,
el directorio `thesis` también aparecerá allí.
Aunque la línea de comandos y el explorador de archivos son dos formas diferentes de interactuar con los archivos,
los archivos y directorios en sí son los mismos.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Buenos nombres para archivos y directorios

Los nombres complicados de archivos y directorios pueden complicar tu vida
cuando trabajas en la línea de comandos. Aquí te ofrecemos algunos consejos útiles
para los nombres de tus archivos y directorios.

1. No utilices espacios.

Los espacios pueden darle más significado a un nombre,
pero como se utilizan para separar argumentos en la línea de comandos,
es mejor evitarlos en los nombres de archivos y directorios.
Puedes usar `-` o `_` en su lugar (por ejemplo, `north-pacific-gyre/` en lugar de `north pacific gyre/`).
Para comprobar esto, intenta escribir `mkdir north pacific gyre` y verifica qué directorio(s) se crean cuando verificas con `ls -F`.

2. No comiences el nombre con `-` (guion).

Los comandos tratan los nombres que comienzan con `-` como opciones.

3. Usa solo letras, números, `.` (punto), `-` (guion) y `_` (guión bajo).

Muchos otros caracteres tienen significados especiales en la línea de comandos.
Aprenderemos sobre algunos de ellos durante esta lección.
Existen caracteres especiales que pueden hacer que tu comando no funcione
como se espera e incluso pueden provocar la pérdida de datos.

Si necesitas referirte a nombres de archivos o directorios con espacios
u otros caracteres especiales, debes encerrar el nombre entre comillas (`""`).

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  instructor

A veces los estudiantes pueden quedar atrapados en editores de texto de línea de comandos
como Vim, Emacs o Nano. Cerrar el emulador de terminal y abrir
uno nuevo puede ser frustrante ya que los estudiantes tendrán que navegar hasta el
directorio correcto nuevamente. Nuestra recomendación para mitigar este problema es que
los instructores usen el mismo editor de texto que los estudiantes durante los talleres
(en la mayoría de los casos Nano).

::::::::::::::::::::::::::::::::::::::::::::::::::

### Crear un archivo de texto

Cambiamos nuestro directorio de trabajo a `thesis` usando `cd`,
y luego ejecutamos un editor de texto llamado Nano para crear un archivo llamado `draft.txt`:

```bash
$ cd thesis
$ nano draft.txt
```

:::::::::::::::::::::::::::::::::::::::::  callout

## ¿Qué editor?

Cuando decimos que '`nano` es un editor de texto', realmente nos referimos a 'texto'. Solo puede
trabajar con datos de caracteres simples, no con tablas, imágenes ni ningún otro
medio amigable para el ser humano. Lo utilizamos en los ejemplos porque es uno de los
editores de texto menos complejos. Sin embargo, debido a esta característica, es posible
que no sea lo suficientemente potente o flexible para el trabajo que debes hacer
después de este curso. En sistemas Unix (como Linux y macOS),
muchos programadores usan [Emacs](https://www.gnu.org/software/emacs/) o
[Vim](https://www.vim.org/) (ambos requieren más tiempo para aprender),
o un editor gráfico como [Gedit](https://projects.gnome.org/gedit/)
o [VScode](https://code.visualstudio.com/). En Windows, puede que desees utilizar
[Notepad++](https://notepad-plus-plus.org/). Windows también tiene un editor incorporado
llamado `notepad` que se puede ejecutar desde la línea de comandos de la misma
forma que `nano` para los propósitos de esta lección.

No importa qué editor uses, tendrás que saber dónde busca
y guarda archivos. Si lo inicias desde la línea de comandos, (probablemente)
utilizará el directorio de trabajo actual como su ubicación predeterminada. Si usas
el menú de inicio de tu computadora, es posible que desee guardar archivos en su escritorio o
directorio Documentos en su lugar. Puedes cambiar esto navegando a
otro directorio la primera vez que hagas clic en 'Guardar como...'

::::::::::::::::::::::::::::::::::::::::::::::::::

Escribamos algunas líneas de texto.

![](fig/nano-screenshot.png){alt="captura de pantalla del editor de texto nano en acción con el texto Ya no es publicar o perecer, ahora es compartir y prosperar"}

Una vez que estemos satisfechos con nuestro texto, podemos presionar <kbd>Ctrl</kbd>+\+<kbd>O</kbd>
(pulsa la tecla <kbd>Ctrl</kbd> o <kbd>Control</kbd> y, mientras
la mantienes presionada, presiona la tecla <kbd>O</kbd>) para escribir nuestros datos en el disco. Nos pedirán
que proporcionemos un nombre para el archivo que contendrá nuestro texto. Presiona <kbd>Return</kbd> para aceptar
la opción predeterminada sugerida: `draft.txt`.

Una vez que se guarda nuestro archivo, podemos usar <kbd>Ctrl</kbd>+\+<kbd>X</kbd> para salir del editor y
volver al shell.

:::::::::::::::::::::::::::::::::::::::::  callout

## Control, Ctrl o tecla ^

La tecla Control también se llama tecla 'Ctrl'. Hay varias formas
en las que se describe el uso de la tecla de Control. Por ejemplo, puedes
ver una instrucción para presionar la tecla <kbd>Control</kbd> y, mientras la mantienes presionada,
presionar la tecla <kbd>X</kbd>, que se describe a través de alguno de estos ejemplos:

- `Control-X`
- `Control+X`
- `Ctrl-X`
- `Ctrl+X`
- `^X`
- `C-x`

En nano, a lo largo de la parte inferior de la pantalla verás `^G Get Help ^O WriteOut`.
Esto significa que puedes usar `Control-G` para solicitar ayuda y `Control-O` para guardar tu
archivo.

::::::::::::::::::::::::::::::::::::::::::::::::::

`nano` no deja ninguna salida en la pantalla cuando sale,
pero `ls` ahora muestra que hemos creado un archivo llamado `draft.txt`:

```bash
$ ls
```

```output
draft.txt
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Creando Archivos de Otra Manera

Hemos visto cómo crear archivos de texto utilizando el editor `nano`.
Ahora, prueba el siguiente comando:

```bash
$ touch my_file.txt
```

1. ¿Qué hizo el comando `touch`?
  Al mirar tu directorio actual utilizando el explorador de archivos gráfico,
  ¿aparece el archivo?

2. Utiliza `ls -l` para inspeccionar los archivos. ¿Qué tamaño tiene `my_file.txt`?

3. ¿Cuándo podrías querer crear un archivo de esta manera?

:::::::::::::::  solution

## Solución

1. El comando `touch` genera un nuevo archivo llamado `my_file.txt` en
  tu directorio actual. Puedes observar este archivo recién creado escribiendo `ls` enel
  indicador de línea de comandos. `my_file.txt` también se puede ver en el explorador de archivos gráfico.

2. Al inspeccionar el archivo con `ls -l`, se observa que el tamaño de `my_file.txt` es de 0 bytes.
  En otras palabras, no contiene datos. Si abres `my_file.txt` utilizando tu editor de texto, estará en blanco.

3. Algunos programas no generan archivos de salida, sino que requieren que los archivos vacíos ya se hayan generado.
  Cuando se ejecuta el programa, busca un archivo existente para
  poblar con su salida. El comando `touch` te permite
  generar eficientemente un archivo de texto en blanco para ser utilizado por estos
  programas.

:::::::::::::::::::::::::

Para evitar confusiones más adelante,
sugerimos eliminar el archivo que acabas de crear antes de continuar con el resto del episodio, de lo contrario, las salidas futuras pueden ser diferentes a las indicadas en la lección.
Para hacer esto, utiliza el siguiente comando:

```bash
$ rm my_file.txt
```

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## ¿Qué hay en un nombre?

Es posible que hayas notado que todos los archivos de Nelle se llaman 'algo punto algo', y en esta parte de la lección, siempre usamos la extensión `.txt`. Esto es solo una convención. Podemos llamar a un archivo `mythesis` o casi cualquier otra cosa. Sin embargo, la mayoría de las personas usan nombres de dos partes la mayor parte del tiempo para ayudarlos (y a sus programas) a diferenciar diferentes tipos de archivos. El segunda parte de ese nombre se llama **extensión de archivo** e indica qué tipo de datos contiene el archivo: `.txt` representa un archivo de texto sin formato, `.pdf` indica un documento PDF, `.cfg` es un archivo de configuración lleno de parámetros para algún programa u otro, `.png` es una imagen PNG, y así sucesivamente.

Esto es solo una convención, aunque importante. Los archivos simplemente contienen bytes; depende de nosotros y nuestros programas interpretar esos bytes según las reglas para archivos de texto sin formato, documentos PDF, archivos de configuración, imágenes, etc.

Si llamamos a una imagen PNG de una ballena como `whale.mp3`, no la convertimos automáticamente en una grabación de canto de ballena, aunque *podría* hacer que el sistema operativo asocie el archivo con un programa reproductor de música. En este caso, si alguien hace doble clic en `whale.mp3` en un programa de explorador de archivos, el reproductor de música intentará abrir el archivo `whale.mp3` (y erróneamente intentará abrirlo como una grabación de ballena).

::::::::::::::::::::::::::::::::::::::::::::::::::

## Moviendo archivos y directorios

Volviendo al directorio `shell-lesson-data/exercise-data/writing`,

```bash
$ cd ~/Desktop/shell-lesson-data/exercise-data/writing
```

En nuestro directorio `thesis` tenemos un archivo `draft.txt`
que no es un nombre especialmente informativo,
así que cambiemos el nombre del archivo usando `mv`,
que significa 'mover':

```bash
$ mv thesis/draft.txt thesis/quotes.txt
```

El primer argumento le dice a `mv` qué estamos 'moviendo',
mientras que el segundo argumento es dónde debe ir.
En este caso,
estamos moviendo `thesis/draft.txt` a `thesis/quotes.txt`,
lo que tiene el mismo efecto que cambiarle el nombre al archivo.
Efectivamente,
`ls` nos muestra que `thesis` ahora contiene un único archivo llamado `quotes.txt`:

```bash
$ ls thesis
```

```output
quotes.txt
```

Uno debe tener cuidado al especificar el nombre del archivo de destino, ya que `mv` sobrescribirá silenciosamente cualquier archivo existente con el mismo nombre, lo que podría provocar la pérdida de datos. De forma predeterminada, `mv` no solicitará confirmación antes de sobrescribir los archivos. Sin embargo, una opción adicional, `mv -i` (o `mv --interactive`), solicitará confirmación para la sobrescritura.

Ten en cuenta que `mv` también funciona en directorios.

Moveremos `quotes.txt` al directorio de trabajo actual.
Usaremos `mv` una vez más,
pero esta vez usaremos solo el nombre de un directorio como segundo argumento
para decirle a `mv` que queremos conservar el nombre de archivo
pero poner el archivo en algún lugar nuevo.
(Es por eso que el comando se llama 'mover'.)
En este caso,
el nombre de directorio que usamos es el nombre especial de directorio `.` que mencionamos anteriormente.

```bash
$ mv thesis/quotes.txt .
```

El efecto es mover el archivo del directorio en el que se encontraba al directorio de trabajo actual.
`ls` ahora nos muestra que `thesis` está vacío:

```bash
$ ls thesis
```

```output
$
```

Alternativamente, podemos confirmar que el archivo `quotes.txt` ya no está presente en el directorio `thesis`
intentando listar explícitamente:

```bash
$ ls thesis/quotes.txt
```

```error
ls: cannot access 'thesis/quotes.txt': No such file or directory
```

`ls` con un nombre de archivo o directorio como argumento solo muestra el archivo o directorio solicitado.
Si el archivo dado como argumento no existe, el shell devuelve un error como el que vimos anteriormente.
Podemos usar esto para ver que `quotes.txt` ahora está presente en nuestro directorio actual:

```bash
$ ls quotes.txt
```

```output
quotes.txt
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Mover archivos a una nueva carpeta

Después de ejecutar los siguientes comandos,
Jamie se da cuenta de que colocó los archivos `sucrose.dat` y `maltose.dat` en la carpeta equivocada.
Los archivos deberían haber estado en la carpeta `raw`.

```bash
$ ls -F
 analyzed/ raw/
$ ls -F analyzed
fructose.dat glucose.dat maltose.dat sucrose.dat
$ cd analyzed
```

Rellena los espacios en blanco para mover estos archivos a la carpeta `raw/`
(es decir, la que olvidó agregar).

```bash
$ mv sucrose.dat maltose.dat ____/____
```

:::::::::::::::  solution

## Solución

```bash
$ mv sucrose.dat maltose.dat ../raw
```

Recuerda que `..` se refiere al directorio superior (uno por encima del directorio actual)
y que `.` se refiere al directorio actual.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Copiando archivos y directorios

El comando `cp` funciona de manera muy similar a `mv`,
excepto que copia un archivo en lugar de moverlo.
Podemos verificar que hizo lo correcto usando `ls`
con dos rutas como argumentos --- al igual que la mayoría de los comandos Unix,
`ls` puede recibir múltiples rutas a la vez:

```bash
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
```

```output
quotes.txt   thesis/quotations.txt
```

También podemos copiar un directorio y todos sus contenidos usando la opción
[recursiva](https://en.wikipedia.org/wiki/Recursion) `-r`,
por ejemplo, para hacer una copia de seguridad de un directorio:

```bash
$ cp -r thesis thesis_backup
```

Podemos verificar el resultado listando el contenido del directorio `thesis` y `thesis_backup`:

```bash
$ ls thesis thesis_backup
```

```output
thesis:
quotations.txt

thesis_backup:
quotations.txt
```

Es importante incluir la opción `-r`. Si deseas copiar un directorio y omites esta opción,
verás un mensaje que indica que se omitió el directorio porque no se especificó `-r`.

``` bash
$ cp thesis thesis_backup
cp: -r not specified; omitting directory 'thesis'
```


:::::::::::::::::::::::::::::::::::::::  challenge

## Renombrar archivos

Supongamos que creaste un archivo de texto sin formato en tu directorio actual para contener una lista de las
pruebas estadísticas que tendrás que realizar para analizar tus datos y lo nombraste `statstics.txt`

¡Después de crear y guardar este archivo te das cuenta de que cometiste un error ortográfico en el nombre! Quieres
corregir el error, ¿qué comando(s) cubiertos en esta lección necesita ejecutar
para que los comandos siguientes produzcan el resultado que se muestra?

```bash
$ ls -F
```

```output
analyzed/ raw/
```

```bash
$ ls analyzed
```

```output
fructose.dat    sucrose.dat
```

:::::::::::::::  solution

## Solución

```output
rm: remove regular file 'thesis_backup/quotations.txt'? y
```

Al ejecutar el comando `rm -i thesis_backup/quotations.txt` se muestra el mensaje:
`rm: ¿eliminar archivo regular 'thesis_backup/quotations.txt'? y`

La opción `-i` solicita una confirmación antes de eliminar (cada) archivo (usa <kbd>Y</kbd> para confirmar la eliminación
o <kbd>N</kbd> para conservar el archivo).
La línea de comandos no tiene una papelera de reciclaje de la que se puedan recuperar los archivos eliminados (aunque la mayoría de las interfaces gráficas de Unix sí la tienen). En su lugar, cuando eliminamos archivos, se desvinculan del sistema de archivos para que su espacio de almacenamiento en disco pueda reciclarse. Sí existen herramientas para buscar y recuperar archivos eliminados, pero no hay garantía de que funcionen en cualquier situación en particular, ya que la computadora puede reciclar el espacio en disco del archivo de inmediato.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Usar `rm` de manera segura

¿Qué sucede cuando ejecutamos `rm -i thesis_backup/quotations.txt`?
¿Por qué querríamos esta protección al usar `rm`?

:::::::::::::::::::::::::::::::::::::::  solution

## Solución

```output
rm: remove regular file 'thesis_backup/quotations.txt'? y
```

La opción `-i` solicitará confirmación antes de (cada) eliminación (usa <kbd>Y</kbd> para confirmar la eliminación
o <kbd>N</kbd> para conservar el archivo).
La línea de comandos de Unix no tiene una papelera de reciclaje, por lo que todos los archivos eliminados desaparecerán para siempre. Al usar la opción `-i`, tenemos la oportunidad de verificar que estamos eliminando solo los archivos que deseamos eliminar.

::::::::::::::::::::::::::::::::::::::::::::::::::

Si intentamos eliminar el directorio `thesis` usando `rm thesis`,
obtenemos un mensaje de error:

```bash
$ rm thesis
```

```error
rm: cannot remove 'thesis': Is a directory
```

Esto sucede porque `rm` por defecto solo trabaja con archivos, no con directorios.

`rm` puede eliminar un directorio *y todos sus contenidos* si usamos la
opción recursiva `-r`, y lo hará *sin solicitar ninguna confirmación*:

```bash
$ rm -r thesis
```

Dado que no hay forma de recuperar archivos eliminados utilizando la línea de comandos,
`rm -r` *debe usarse con mucho cuidado*
(es posible que desees agregar la opción interactiva `rm -r -i`).

## Operaciones con múltiples archivos y directorios

Muchas veces se necesita copiar o mover varios archivos a la vez.
Esto se puede hacer proporcionando una lista de nombres de archivos individuales
o especificando un patrón de nombres utilizando comodines. Los comodines son
caracteres especiales que se utilizan para representar caracteres desconocidos
o conjuntos de caracteres cuando se navega por el sistema de archivos de Unix.

:::::::::::::::::::::::::::::::::::::::  challenge

## Copiar con Múltiples Nombres de Archivo

En el siguiente ejemplo, ¿qué hace `cp` cuando se le dan varios nombres de archivo y un nombre de directorio?

```bash
$ mkdir backup
$ cp creatures/minotaur.dat creatures/unicorn.dat backup/
```

En el siguiente ejemplo, ¿qué hace `cp` cuando se le dan tres o más nombres de archivo?

```bash
$ cd creatures
$ ls -F
```

```output
basilisk.dat  minotaur.dat  unicorn.dat
```

```bash
$ cp minotaur.dat unicorn.dat basilisk.dat
```

:::::::::::::::::::::::::::::::::::::::  solution

## Solución

Si se le dan más de un nombre de archivo seguido de un nombre de directorio
(es decir, el directorio de destino debe ser el último argumento),
`cp` copiará los archivos al directorio especificado.

Si se le dan tres nombres de archivo, `cp` lanzará un error como el que se muestra a continuación,
porque está esperando un nombre de directorio como último argumento.

```error
cp: target 'basilisk.dat' is not a directory
```

::::::::::::::::::::::::::::::::::::::::::::::::::

### Usando comodines para acceder a varios archivos a la vez

:::::::::::::::::::::::::::::::::::::::::  callout

## Comodines

`*` es un **comodín**, que representa cero o más caracteres en otro nombre de archivo. Así que `*.txt` representa `ethane.pdb`, `propane.pdb` y todos
los archivos que terminan con '.pdb'. Por otro lado, `p*.pdb` solo representa
`pentane.pdb` y `propane.pdb`, porque la 'p' al inicio solo puede
representar nombres de archivo que comienzan con la letra 'p'.

`?` también es un comodín, pero representa exactamente un carácter.
Entonces, `?ethane.pdb` podría representar `methane.pdb`, mientras
que `*ethane.pdb` representa tanto `ethane.pdb` como `methane.pdb`.

Los comodines se pueden utilizar en combinación entre sí. Por ejemplo,
`???ane.pdb` indica tres caracteres seguidos de `ane.pdb`,
lo que nos dará `cubane.pdb  ethane.pdb  octane.pdb`.


Cuando el shell encuentra un comodín, expande el comodín para crear una
lista de nombres de archivo coincidentes *antes* de que se ejecute el comando anterior.
Como excepción, si una expresión de comodín no coincide
con ningún archivo, Bash pasará la expresión como un argumento al comando
tal cual aparece. Por ejemplo, escribir `ls *.pdf` en el directorio `alkanes`
(que contiene solo archivos con nombres que terminan en `.pdb`) resulta en
un mensaje de error de que no hay un archivo llamado `*.pdf`.
Sin embargo, generalmente comandos como `wc` y `ls` ven las listas de
nombres de archivo que coinciden con esas expresiones, pero no los comodines
mismos. Es el shell, no otros programas, el que expande
los comodines.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Listar nombres de archivo que coinciden con un patrón

¿Cuál de los siguientes comandos `ls` producirá esta salida cuando se ejecute en el directorio `alkanes`?

`ethane.pdb   methane.pdb`

1. `ls *t*ane.pdb`
2. `ls *t?ne.*`
3. `ls *t??ne.pdb`
4. `ls ethane.*`

:::::::::::::::  solution

## Solución

La solución es `3.`

`1.` muestra todos los archivos cuyos nombres contienen cero o más caracteres (`*`) seguidos de la letra `t`,
luego cero o más caracteres (`*`) seguidos de `ane.pdb`.
Esto nos da como resultado `ethane.pdb  methane.pdb  octane.pdb  pentane.pdb`.

`2.` muestra todos los archivos cuyos nombres comienzan con cero o más caracteres (`*`) seguidos de
la letra `t`, luego un solo carácter (`?`), luego `ne.` seguido de cero o más caracteres (`*`).
Esto nos dará `octane.pdb` y `pentane.pdb` pero no coincide con nada
que termine en `thane.pdb`.

`3.` soluciona los problemas de la opción 2 al coincidir dos caracteres (`??`) entre `t` y `ne`.
Esta es la solución.

`4.` solo muestra archivos que comienzan con `ethane.`.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Más sobre comodines

Sam tiene un directorio que contiene datos de calibración, conjuntos de datos y descripciones de los conjuntos de datos:

```bash
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   └── datasets
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    └── all_november_files
```

Antes de irse a otro viaje de campo, quiere hacer una copia de seguridad de sus datos y
enviar algunos conjuntos de datos a su colega Bob. Sam utiliza los siguientes comandos
para hacer el trabajo:

```bash
$ cp *dataset* backup/datasets
$ cp ____calibration____ backup/calibration
$ cp 2015-____-____ send_to_bob/all_november_files/
$ cp ____ send_to_bob/all_datasets_created_on_a_23rd/
```

Ayuda a Sam llenando los espacios en blanco.

La estructura final del directorio debería verse así:

```bash
.
├── 2015-10-23-calibration.txt
├── 2015-10-23-dataset1.txt
├── 2015-10-23-dataset2.txt
├── 2015-10-23-dataset_overview.txt
├── 2015-10-26-calibration.txt
├── 2015-10-26-dataset1.txt
├── 2015-10-26-dataset2.txt
├── 2015-10-26-dataset_overview.txt
├── 2015-11-23-calibration.txt
├── 2015-11-23-dataset1.txt
├── 2015-11-23-dataset2.txt
├── 2015-11-23-dataset_overview.txt
├── backup
│   ├── calibration
│   │   ├── 2015-10-23-calibration.txt
│   │   ├── 2015-10-26-calibration.txt
│   │   └── 2015-11-23-calibration.txt
│   └── datasets
│       ├── 2015-10-23-dataset1.txt
│       ├── 2015-10-23-dataset2.txt
│       ├── 2015-10-23-dataset_overview.txt
│       ├── 2015-10-26-dataset1.txt
│       ├── 2015-10-26-dataset2.txt
│       ├── 2015-10-26-dataset_overview.txt
│       ├── 2015-11-23-dataset1.txt
│       ├── 2015-11-23-dataset2.txt
│       └── 2015-11-23-dataset_overview.txt
└── send_to_bob
    ├── all_datasets_created_on_a_23rd
    │   ├── 2015-10-23-dataset1.txt
    │   ├── 2015-10-23-dataset2.txt
    │   ├── 2015-10-23-dataset_overview.txt
    │   ├── 2015-11-23-dataset1.txt
    │   ├── 2015-11-23-dataset2.txt
    │   └── 2015-11-23-dataset_overview.txt
    └── all_november_files
        ├── 2015-11-23-calibration.txt
        ├── 2015-11-23-dataset1.txt
        ├── 2015-11-23-dataset2.txt
        └── 2015-11-23-dataset_overview.txt
```

:::::::::::::::  solution

## Solución

```bash
$ cp *calibration.txt backup/calibration
$ cp 2015-11-* send_to_bob/all_november_files/
$ cp *-23-dataset* send_to_bob/all_datasets_created_on_a_23rd/
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::: setSizeAll summarizes the size of the files and subdirectories inside a directory. It can accept an optional argument to specify a different format for the sizes. See examples below:
```bash
$ setSizeAll
```

	Output:

```output
400
```

```bash
$ setSizeAll -h
```

	Output:

```output
1.15K
```

```bash
$ setSizeAll -m
```

	Output:

```output
0.00M
```

