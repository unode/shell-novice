---
title: Presentando el Shell
teaching: 5
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objetivos

- Explicar cómo se relaciona el shell con el teclado, la pantalla, el sistema operativo y los programas de los usuarios.
- Explicar cuándo y por qué se deben usar interfaces de línea de comandos en lugar de interfaces gráficas.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: preguntas

- ¿Qué es un shell de comandos y por qué lo usaría?

::::::::::::::::::::::::::::::::::::::::::::::::::

### Contexto

Los humanos y las computadoras interactúan comúnmente de muchas formas diferentes, como a través de un teclado y un ratón, interfaces de pantalla táctil o usando sistemas de reconocimiento de voz.
La forma más utilizada de interactuar con las computadoras personales se llama **interfaz gráfica de usuario** (GUI, por sus siglas en inglés).
Con una GUI, damos instrucciones haciendo clic con el mouse y utilizando interacciones basadas en menús.

Si bien la ayuda visual de una GUI facilita su aprendizaje, la entrega de instrucciones a una computadora de esta manera no escala muy bien.
Imagínese la siguiente tarea:
para una búsqueda literaria, debe copiar la tercera línea de mil archivos de texto en mil directorios diferentes y pegarla en un solo archivo.
Usando una GUI, no solo estaría haciendo clic en su escritorio durante varias horas, sino que también podría cometer un error en el proceso de completar esta tarea repetitiva.
Aquí es donde aprovechamos el shell de Unix.
El shell de Unix es tanto una **interfaz de línea de comandos** (CLI) como un lenguaje de script, lo que permite realizar tareas repetitivas automáticamente y rápidamente.
Con los comandos adecuados, el shell puede repetir tareas con o sin alguna modificación tantas veces como queramos.
Usando el shell, la tarea en el ejemplo literario se puede realizar en cuestión de segundos.

### El Shell

El shell es un programa donde los usuarios pueden escribir comandos.
Con el shell, es posible invocar programas complicados como software de modelado climático o comandos simples que crean un directorio vacío con solo una línea de código.
El shell de Unix más popular es Bash (Bourne Again SHell, que significa "nacido nuevamente de una shell escrita por Stephen Bourne").
Bash es el shell predeterminado en la mayoría de las implementaciones modernas de Unix y en la mayoría de los paquetes que proporcionan herramientas similares a Unix para Windows.
Tenga en cuenta que 'Git Bash' es un software que permite a los usuarios de Windows utilizar una interfaz similar a Bash al interactuar con Git.

Usar el shell requerirá un esfuerzo y un tiempo para aprender.
Mientras que una GUI presenta opciones para seleccionar, las opciones de la CLI no se presentan automáticamente,
por lo que debe aprender algunos comandos como un nuevo vocabulario en un idioma que está estudiando.
Sin embargo, a diferencia de un idioma hablado, un pequeño número de "palabras" (es decir, comandos) lo llevará muy lejos,
y hoy cubriremos esos pocos esenciales.

La gramática de un shell le permite combinar herramientas existentes en tuberías poderosas y manejar grandes volúmenes de datos automáticamente. Las secuencias de comandos pueden escribirse en un *script*, mejorando la reproducibilidad de los flujos de trabajo.

Además, la línea de comandos es a menudo la forma más fácil de interactuar con máquinas remotas y supercomputadoras.
La familiaridad con el shell es fundamental para ejecutar una variedad de herramientas y recursos especializados, incluidos los sistemas informáticos de alto rendimiento.
A medida que los clústeres y los sistemas de computación en la nube se vuelven más populares para el procesamiento de datos científicos, la capacidad de interactuar con el shell se está convirtiendo en una habilidad necesaria.
Podemos aprovechar las habilidades de línea de comandos cubiertas aquí para abordar una amplia gama de preguntas científicas y desafíos computacionales.

Comencemos.

Cuando se abre el shell por primera vez, se muestra un **prompt** (indicador),
lo que indica que el shell está esperando una entrada.

```bash
$
```

El prompt utiliza típicamente `$` como indicador, pero puede usar un símbolo diferente.
En los ejemplos de esta lección, mostraremos el prompt como `$`.
Lo más importante es *no escribir el prompt* al escribir comandos.
Solo se debe escribir el comando que sigue al prompt.
Esta regla se aplica tanto en estas lecciones como en las lecciones de otras fuentes.
También tenga en cuenta que después de escribir un comando, debe presionar la tecla <kbd>Enter</kbd> para ejecutarlo.

El prompt va seguido de un **cursor de texto**, un carácter que indica la posición donde aparecerá lo que escriba.
El cursor suele ser un bloque parpadeante o sólido, pero también puede ser un guion bajo o una barra vertical.
Tal vez lo haya visto en un programa editor de texto, por ejemplo.

Tenga en cuenta que su prompt puede verse un poco diferente. En particular, la mayoría de los entornos de shell populares por defecto colocan su nombre de usuario y el nombre del host antes del `$`. Un prompt así podría verse, por ejemplo:

```bash
nelle@localhost $
```

El prompt incluso puede incluir más que esto. No se preocupe si su prompt no es solo un breve `$`. Esta lección no depende de esta información adicional y tampoco debería interferir. Lo único importante en lo que debemos enfocarnos es el carácter `$` y veremos más adelante por qué.

Así que probemos nuestro primer comando, `ls`, que es la abreviatura de *listar*.
Este comando mostrará el contenido del directorio actual:

```bash
$ ls
```

```output
Desktop     Downloads   Movies      Pictures
Documents   Library     Music       Public
```

:::::::::::::::::::::::::::::::::::::::::  callout

## Comando no encontrado

Si el shell no puede encontrar un programa cuyo nombre es el comando que escribió, mostrará un mensaje de error como este:

```bash
$ ks
```

```output
ks: command not found
```

Esto puede suceder si el comando fue escrito incorrectamente o si el programa correspondiente a ese comando no está instalado.


::::::::::::::::::::::::::::::::::::::::::::::::::

## El Pipeline de Nelle: Un problema típico

Nelle Nemo, una bióloga marina,
acaba de regresar de un estudio de seis meses del
[giro del Pacífico Norte](https://en.wikipedia.org/wiki/North_Pacific_Gyre),
donde ha estado muestreando vida marina gelatinosa en el
[Remolino de Basura del Pacífico](https://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
Tiene 1520 muestras que ha analizado utilizando una máquina de ensayo para medir la abundancia relativa de 300 proteínas.
Necesita procesar estos 1520 archivos utilizando un programa imaginario llamado `goostats.sh`.
Además de esta gran tarea, debe redactar los resultados antes de que termine el mes, para que su artículo pueda aparecer en un número especial de la revista *Aquatic Goo Letters*.

Si Nelle elige ejecutar `goostats.sh` manualmente utilizando una GUI,
tendrá que seleccionar y abrir un archivo 1520 veces.
Si `goostats.sh` tarda 30 segundos en ejecutar cada archivo, todo el proceso le llevará más de 12 horas de atención a Nelle.
Con el shell, Nelle puede asignarle esta tarea mundana a su computadora mientras ella se concentra en escribir su artículo.

Las próximas lecciones explorarán las formas en que Nelle puede lograr esto.
Más específicamente,
las lecciones explicarán cómo puede usar el shell de comandos para ejecutar el programa `goostats.sh`,
utilizando bucles para automatizar los pasos repetitivos de ingresar los nombres de los archivos,
para que su computadora pueda trabajar mientras ella escribe su artículo.

Como bono adicional,
una vez que haya creado un pipeline de procesamiento,
podrá utilizarlo nuevamente cada vez que recolecte más datos.

Para lograr su tarea, Nelle necesita saber cómo:

- navegar a un archivo o directorio
- crear un archivo o directorio
- verificar la longitud de un archivo
- concatenar comandos
- obtener un conjunto de archivos
- iterar sobre archivos
- ejecutar un script de shell que contenga su pipeline



:::::::::::::::::::::::::::::::::::::::: puntos clave

- Un shell es un programa cuyo propósito principal es leer comandos y ejecutar otros programas.
- Esta lección utiliza Bash, el shell predeterminado en muchas implementaciones de Unix.
- Los programas se pueden ejecutar en Bash ingresando comandos en el prompt de línea de comandos.
- Las principales ventajas del shell son su alta relación acción-tecla, su capacidad para automatizar tareas repetitivas y su capacidad para acceder a máquinas en red.
- Un desafío importante al usar el shell puede ser saber qué comandos deben ejecutarse y cómo ejecutarlos.

::::::::::::::::::::::::::::::::::::::::::::::::::