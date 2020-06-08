Expansión
Cada vez que escribe una línea de comando y presiona la tecla Intro, bash realiza varios procesos sobre el texto antes de ejecutar su comando. Hemos visto un par de casos de cómo una secuencia de caracteres simple, por ejemplo "*", puede tener mucho significado para el shell. El proceso que hace que esto suceda se llama expansión . Con la expansión, escribe algo y se expande en otra cosa antes de que el shell actúe sobre él. Para demostrar lo que queremos decir con esto, echemos un vistazo al comando echo . echo es un shell integrado que realiza una tarea muy simple. Imprime sus argumentos de texto en la salida estándar:

[me @ linuxbox me] $ echo esta es una prueba
esta es una prueba

Eso es bastante sencillo. Cualquier argumento pasado a echo se muestra. Probemos con otro ejemplo:

[me @ linuxbox me] $ echo *
Documentos de escritorio ls-output.txt Música Imágenes Plantillas públicas Videos

Entonces, ¿qué acaba de pasar? ¿Por qué no se hizo eco de la impresión "*"? Como recordará de nuestro trabajo con comodines, el carácter "*" significa que coincide con cualquier carácter en un nombre de archivo, pero lo que no vimos en nuestra discusión original fue cómo lo hace el shell. La respuesta simple es que el shell expande el "*" en otra cosa (en este caso, los nombres de los archivos en el directorio de trabajo actual) antes de que se ejecute el comando echo . Cuando se presiona la tecla Intro, el shell expande automáticamente los caracteres que califican en la línea de comando antes de que se ejecute el comando, por lo que el comando echo nunca vio el "*", solo su resultado expandido. Sabiendo esto, podemos ver que el eco se comportó como se esperaba.

Expansión de nombre de ruta
El mecanismo por el cual funcionan los comodines se denomina expansión de nombre de ruta . Si probamos algunas de las técnicas que empleamos en nuestras lecciones anteriores, veremos que realmente son expansiones. Dado un directorio de inicio que se ve así:

[me @ linuxbox me] $ ls

Escritorio
ls-output.txt
Documentos de música
Imágenes
Público
Plantillas
Videos
Podríamos llevar a cabo las siguientes expansiones:

[me @ linuxbox me] $ echo D *
Documentos de escritorio

y:

[me @ linuxbox me] $ echo * s
Documentos Imágenes Plantillas Videos

o incluso:

[me @ linuxbox me] $ echo [[: upper:]] *
Documentos de escritorio Música Imágenes Plantillas públicas Videos

y mirando más allá de nuestro directorio de inicio:

[me @ linuxbox me] $ echo / usr / * / share
/ usr / kerberos / share / usr / local / share

Expansión Tilde
Como recordará de nuestra introducción al comando cd , el carácter tilde ("~") tiene un significado especial. Cuando se usa al comienzo de una palabra, se expande en el nombre del directorio de inicio del usuario nombrado, o si no se nombra a ningún usuario, el directorio de inicio del usuario actual:

[me @ linuxbox me] $ echo ~
/ home / me

Si el usuario "foo" tiene una cuenta, entonces:

[yo @ linuxbox me] $ echo ~ foo
/ home / foo

Expansión Aritmética
El caparazón permite que la aritmética se realice por expansión. Esto nos permite usar el indicador de comandos de la shell como calculadora:

[me @ linuxbox me] $ echo $ ((2 + 2))
4

La expansión aritmética usa la forma:

	$ ((expresión))
donde expresión es una expresión aritmética que consiste en valores y operadores aritméticos.

La expansión aritmética solo admite números enteros (números enteros, sin decimales), pero puede realizar una gran cantidad de operaciones diferentes.

Los espacios no son significativos en las expresiones aritméticas y las expresiones pueden estar anidadas. Por ejemplo, para multiplicar cinco al cuadrado por tres:

[me @ linuxbox me] $ echo $ (($ ((5 ** 2)) * 3))
75

Los paréntesis individuales pueden usarse para agrupar subexpresiones múltiples. Con esta técnica, podemos reescribir el ejemplo anterior y obtener el mismo resultado usando una sola expansión en lugar de dos:

[me @ linuxbox me] $ echo $ (((5 ** 2) * 3))
75

Aquí hay un ejemplo usando los operadores de división y resto. Observe el efecto de la división de enteros:

[me @ linuxbox me] $ echo Cinco dividido entre dos es igual a $ ((5/2))
Cinco dividido entre dos es igual a 2
[me @ linuxbox me] $ echo con $ ((5% 2)) sobrante.
con 1 sobrante.

Expansión de abrazaderas
Quizás la expansión más extraña se llama expansión de llaves . Con él, puede crear múltiples cadenas de texto a partir de un patrón que contiene llaves. Aquí hay un ejemplo:

[me @ linuxbox me] $ echo Frente- {A, B, C} -Atrás
Frente-A-Atrás Frente-B-Atrás Frente-C-Atrás

Los patrones que se expandirán entre paréntesis pueden contener una parte inicial llamada preámbulo y una parte posterior llamada postdata . La expresión de llave en sí misma puede contener una lista de cadenas separadas por comas o un rango de enteros o caracteres individuales. El patrón puede no contener espacios en blanco incrustados. Aquí hay un ejemplo usando un rango de enteros:

[me @ linuxbox me] $ echo Número_ {1..5}
Número_1 Número_2 Número_3 Número_4 Número_5

Un rango de letras en orden inverso:

[me @ linuxbox me] $ echo {Z..A}
ZYXWVUTSRQPONMLKJIHGF EDCBA

Las expansiones de llaves pueden estar anidadas:

[me @ linuxbox me] $ echo a {A {1,2}, B {3,4}} b
aA1b aA2b aB3b aB4b

Entonces, ¿para qué sirve esto? La aplicación más común es hacer listas de archivos o directorios que se crearán. Por ejemplo, si fuera fotógrafo y tuviera una gran colección de imágenes que desea organizar en años y meses, lo primero que puede hacer es crear una serie de directorios con el nombre en formato numérico "Año-Mes". De esta manera, los nombres de directorio se ordenarán en orden cronológico. Puede escribir una lista completa de directorios, pero eso es mucho trabajo y también es propenso a errores. En cambio, podrías hacer esto:

[me @ linuxbox me] $ mkdir Photos
[me @ linuxbox me] $ cd Photos
[me @ linuxbox Photos] $ mkdir {2007..2009} -0 {1..9} {2007..2009} - {10. .12}
[me @ linuxbox fotos] $ ls

2007-01 2007-07 2008-01 2008-07 2009-01 2009-07
2007-02 2007-08 2008-02 2008-08 2009-02 2009-08
2007-03 2007-09 2008-03 2008-09 2009-03 2009-09
2007-04 2007-10 2008-04 2008-10 2009-04 2009-10
2007-05 2007-11 2008-05 2008-11 2009-05 2009-11
2007-06 2007-12 2008-06 2008-12 2009-06 2009-12
Bastante hábil!

Expansión de Parámetros
Solo vamos a tocar brevemente la expansión de parámetros en esta lección, pero la cubriremos más adelante. Es una característica que es más útil en los scripts de shell que directamente en la línea de comandos. Muchas de sus capacidades tienen que ver con la capacidad del sistema de almacenar pequeños fragmentos de datos y dar un nombre a cada fragmento. Muchos de estos fragmentos, más propiamente llamados variables , están disponibles para su examen. Por ejemplo, la variable denominada "USUARIO" contiene su nombre de usuario. Para invocar la expansión de parámetros y revelar el contenido del USUARIO, debe hacer esto:

[me @ linuxbox me] $ echo $ USER
me

Para ver una lista de variables disponibles, intente esto:

[yo @ linuxbox me] $ printenv | Menos

Es posible que haya notado que con otros tipos de expansión, si escribe mal un patrón, la expansión no tendrá lugar y el comando echo simplemente mostrará el patrón mal escrito. Con la expansión de parámetros, si escribe mal el nombre de una variable, la expansión seguirá teniendo lugar, pero dará como resultado una cadena vacía:

[me @ linuxbox me] $ echo $ SUER
[me @ linuxbox ~] $

Sustitución de comando
La sustitución de comandos nos permite usar la salida de un comando como una expansión:

[me @ linuxbox me] $ echo $ (ls)
Documentos de escritorio ls-output.txt Música Imágenes Plantillas públicas Videos

Uno de mis favoritos es algo como esto:

[me @ linuxbox me] $ ls -l $ (que cp)
-rwxr-xr-x 1 raíz raíz 71516 05/12/2007 08:58 / bin / cp

Aquí pasamos los resultados de que cp como argumento para el comando ls , obteniendo así la lista del programa cp sin tener que conocer su ruta completa. No estamos limitados a simples comandos. Se pueden usar tuberías enteras (solo se muestra la salida parcial):

[me @ linuxbox me] $ archivo $ (ls / usr / bin / * | grep bin / zip)

/ usr / bin / bunzip2:
/ usr / bin / zip: ELF ejecutable LSB de 32 bits, Intel 80386, versión 1 
(SYSV), vinculado dinámicamente (usa libs compartidas), para GNU / Linux 2.6.15, despojado
/ usr / bin / zipcloak: ELF ejecutable LSB de 32 bits, Intel 80386, versión 1
(SYSV), vinculado dinámicamente (usa libs compartidas), para GNU / Linux 2.6.15, despojado
/ usr / bin / zipgrep: ejecutable del texto del script de shell POSIX
/ usr / bin / zipinfo: ELF ejecutable LSB de 32 bits, Intel 80386, versión 1
(SYSV), vinculado dinámicamente (usa libs compartidas), para GNU / Linux 2.6.15, despojado
/ usr / bin / zipnote: ELF ejecutable LSB de 32 bits, Intel 80386, versión 1
(SYSV), vinculado dinámicamente (usa libs compartidas), para GNU / Linux 2.6.15, despojado
/ usr / bin / zipsplit: ELF ejecutable LSB de 32 bits, Intel 80386, versión 1
(SYSV), vinculado dinámicamente (usa libs compartidas), para GNU / Linux 2.6.15, despojado
En este ejemplo, los resultados de la canalización se convirtieron en la lista de argumentos del comando de archivo. Hay una sintaxis alternativa para la sustitución de comandos en programas de shell más antiguos que también es compatible con bash . Utiliza comillas inversas en lugar del signo de dólar y paréntesis:

[me @ linuxbox me] $ ls -l `which cp`
-rwxr-xr-x 1 root root 71516 05/12/2007 08:58 / bin / cp

Citando
Ahora que hemos visto de cuántas maneras el shell puede realizar expansiones, es hora de aprender cómo podemos controlarlo. Toma por ejemplo:

[me @ linuxbox me] $ echo esta es una prueba
esta es una prueba

o:

[me @ linuxbox me] $ [me @ linuxbox ~] $ echo El total es $ 100.00
El total es 00.00

En el primer ejemplo, la división de palabras por el shell eliminó espacios en blanco adicionales de la lista de argumentos del comando echo. En el segundo ejemplo, la expansión de parámetros sustituyó una cadena vacía por el valor de "$ 1" porque era una variable indefinida. El shell proporciona un mecanismo llamado cita para suprimir selectivamente las expansiones no deseadas.

Doble comillas
El primer tipo de cita que veremos son las comillas dobles. Si coloca texto entre comillas dobles, todos los caracteres especiales utilizados por el shell pierden su significado especial y se tratan como caracteres ordinarios. Las excepciones son "$", "\" (barra invertida) y "" "(comilla invertida). Esto significa que la división de palabras, la expansión del nombre de ruta, la expansión de tilde y la expansión de llaves se suprimen, pero la expansión de parámetros, la expansión aritmética y la sustitución de comandos aún se llevan a cabo. Usando comillas dobles, podemos hacer frente a los nombres de archivo que contienen espacios incrustados. Digamos que usted fue la desafortunada víctima de un archivo llamado two words.txt. Si trató de usar esto en la línea de comando, la división de palabras haría que esto se tratara como dos argumentos separados en lugar del argumento único deseado:

[me @ linuxbox me] $ ls -l dos palabras.txt

ls: no puede acceder a dos: no existe tal archivo o directorio
ls: no se puede acceder a words.txt: no existe tal archivo o directorio
Al usar comillas dobles, puede detener la división de palabras y obtener el resultado deseado; Además, incluso puede reparar el daño:

[me @ linuxbox me] $ ls -l "dos palabras.txt"
-rw-rw-r-- 1 me me 18 2008-02-20 13:03 dos palabras.txt
[me @ linuxbox me] $ mv "dos words.txt "two_words.txt

¡Allí! Ahora no tenemos que seguir escribiendo esas molestas comillas dobles. Recuerde, la expansión de parámetros, la expansión aritmética y la sustitución de comandos todavía tienen lugar entre comillas dobles:

[me @ linuxbox me] $ echo "$ USER $ ((2 + 2)) $ (cal)"

yo 4
Febrero 2008
Su Mo Tu We Th Fr Sa
                1 2
 3 4 5 6 7 8 9
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29
Deberíamos tomarnos un momento para ver el efecto de las comillas dobles en la sustitución de comandos. Primero, veamos un poco más a fondo cómo funciona la división de palabras. En nuestro ejemplo anterior, vimos cómo la división de palabras parece eliminar espacios adicionales en nuestro texto:

[me @ linuxbox me] $ echo esta es una prueba
esta es una prueba

De forma predeterminada, la división de palabras busca la presencia de espacios, tabulaciones y líneas nuevas (caracteres de salto de línea) y los trata como delimitadores entre palabras. Esto significa que los espacios, las pestañas y las líneas nuevas sin comillas no se consideran parte del texto. Solo sirven como separadores. Como separan las palabras en diferentes argumentos, nuestra línea de comando de ejemplo contiene un comando seguido de cuatro argumentos distintos. Si agregamos comillas dobles:

[me @ linuxbox me] $ echo "esta es una prueba"
esta es una prueba

la división de palabras se suprime y los espacios incrustados no se tratan como delimitadores, sino que se convierten en parte del argumento. Una vez que se agregan las comillas dobles, nuestra línea de comando contiene un comando seguido de un solo argumento. El hecho de que las nuevas líneas se consideren delimitadores por el mecanismo de división de palabras provoca un efecto interesante, aunque sutil, en la sustitución de comandos. Considera lo siguiente:

[me @ linuxbox me] $ echo $ (cal)
Febrero de 2008 Su Mo Tu We Th Fr Sa 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29
[me @ linuxbox me] $ echo "$ (cal)"

Febrero 2008
Su Mo Tu We Th Fr Sa
                1 2
 3 4 5 6 7 8 9
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29
En primera instancia, la sustitución de comando sin comillas resultó en una línea de comando que contenía treinta y ocho argumentos. En el segundo, una línea de comando con un argumento que incluye los espacios incrustados y las nuevas líneas.

Comillas simples
Si necesita suprimir todas las expansiones, use comillas simples. Aquí hay una comparación de comillas dobles sin comillas y comillas simples:

[me @ linuxbox me] $ echo text ~ / *. txt {a, b} $ (echo foo) $ ((2 + 2)) $ USER
text /home/me/ls-output.txt ab foo 4 me
[ me @ linuxbox me] $ echo "text ~ / *. txt {a, b} $ (echo foo) $ ((2 + 2)) $ USER"
text ~ / *. txt {a, b} foo 4 me
[ me @ linuxbox me] $ echo 'text ~ / *. txt {a, b} $ (echo foo) $ ((2 + 2)) $ USER'
text ~ / *. txt {a, b} $ (echo foo ) $ ((2 + 2)) $ USUARIO

Como puede ver, con cada nivel sucesivo de citas, se suprimen más y más expansiones.

Personajes que escapan
A veces solo quieres citar un solo personaje. Para hacer esto, puede preceder a un personaje con una barra diagonal inversa, que en este contexto se llama el carácter de escape . A menudo, esto se hace entre comillas dobles para evitar selectivamente una expansión:

[me @ linuxbox me] $ echo "El saldo para el usuario $ USER es: \ $ 5.00"
El saldo para el usuario me es: $ 5.00

También es común usar el escape para eliminar el significado especial de un carácter en un nombre de archivo. Por ejemplo, es posible usar caracteres en nombres de archivos que normalmente tienen un significado especial para el shell. Estos incluirían "$", "!", "&", "" Y otros. Para incluir un carácter especial en un nombre de archivo puede:

[me @ linuxbox me] $ mv bad \ & filename good_filename

Para permitir que aparezca un carácter de barra diagonal inversa, escape escribiendo "\\". Tenga en cuenta que entre comillas simples, la barra invertida pierde su significado especial y se trata como un carácter ordinario.

Más trucos de barra invertida
Si nos fijamos en los hombre páginas para cualquier programa escrito por el proyecto GNU , se dará cuenta de que, además de las opciones de línea de comandos que consisten en un guión y una sola letra, también hay nombres de opción largos que empiezan con dos guiones. Por ejemplo, los siguientes son equivalentes:

ls -r
ls --reverse
       
¿Por qué apoyan ambos? La forma corta es para mecanógrafos perezosos en la línea de comando y la forma larga es principalmente para scripts, aunque algunas opciones pueden ser solo de forma larga. A veces uso opciones oscuras, y encuentro que la forma larga es útil si tengo que revisar un script nuevamente meses después de haberlo escrito. Ver el formulario largo me ayuda a comprender qué hace la opción, lo que me ahorra un viaje a la página del manual . Un poco más de tipeo ahora, mucho menos trabajo después. La pereza se mantiene.

Como puede sospechar, usar las opciones de forma larga puede hacer que una sola línea de comando sea muy larga. Para combatir este problema, puede usar una barra diagonal inversa para que el shell ignore un carácter de nueva línea como este:

ls -l \
   --contrarrestar \
   legible por humanos
   --tiempo completo
       
Usar la barra invertida de esta manera nos permite incrustar nuevas líneas en nuestro comando. Tenga en cuenta que para que este truco funcione, la nueva línea debe escribirse inmediatamente después de la barra invertida. Si coloca un espacio después de la barra diagonal inversa, se ignorará el espacio, no la nueva línea. Las barras invertidas también se utilizan para insertar caracteres especiales en nuestro texto. Estos se llaman caracteres de escape de barra invertida . Aquí están los más comunes:

Personaje de escape

Nombre

Posibles Usos

\norte

nueva línea

Agregar líneas en blanco al texto

\ t

lengüeta

Insertar pestañas horizontales en el texto

\una

alerta

Hace que su terminal emita un pitido

\\

barra invertida

Inserta una barra invertida

\F

alimentación

Enviar esto a su impresora expulsa la página

El uso de los caracteres de escape de la barra diagonal inversa es muy común. Esta idea apareció por primera vez en el lenguaje de programación C. Hoy, el shell, C ++, perl, python, awk, tcl y muchos otros lenguajes de programación utilizan este concepto. Utilizando el comando echo con la opción -e nos permitirá demostrar:

[me @ linuxbox me] $ echo -e "Insertar varias líneas en blanco \ n \ n \ n"
Insertar varias líneas en blanco



[me @ linuxbox me] $ echo -e "Palabras \ tseparated \ tby \ thorizontal \ ttabs".
Palabras separadas por pestañas horizontales
[me @ linuxbox me] $ echo -e "\ aMi computadora fue \" bip \ "."

Mi computadora hizo un "pitido".

[me @ linuxbox me] $ echo -e "DEL C: \\ WIN2K \\ LEGACY_OS.EXE"

DEL C: \ WIN2K \ LEGACY_OS.EXE
Anterior | Contenidos | Arriba | próximo

© 2000-2020, William E. Shotts, Jr. La copia literal y la distribución de este artículo completo están permitidas en cualquier medio, siempre que se conserve este aviso de copyright.

Linux® es una marca registrada de Linus Torvalds.