Redirección de E / S
En esta lección, exploraremos una potente característica utilizada por muchos programas de línea de comandos llamada redirección de entrada / salida . Como hemos visto, muchos comandos como ls imprimen su salida en la pantalla. Sin embargo, este no tiene que ser el caso. Mediante el uso de algunas notaciones especiales, podemos redirigir la salida de muchos comandos a archivos, dispositivos e incluso a la entrada de otros comandos.

Salida estándar
La mayoría de los programas de línea de comandos que muestran sus resultados lo hacen enviando sus resultados a una instalación llamada salida estándar . Por defecto, la salida estándar dirige su contenido a la pantalla. Para redirigir la salida estándar a un archivo, el carácter ">" se usa así:

[me @ linuxbox me] $ ls> file_list.txt

En este ejemplo, el comando ls se ejecuta y los resultados se escriben en un archivo llamado file_list.txt. Como la salida de ls fue redirigida al archivo, no aparecen resultados en la pantalla.

Cada vez que se repite el comando anterior, file_list.txt se sobrescribe desde el principio con la salida del comando ls . Si desea que los nuevos resultados se agreguen al archivo, use ">>" de esta manera:

[me @ linuxbox me] $ ls >> file_list.txt

Cuando se añaden los resultados, los nuevos resultados se agregan al final del archivo, lo que hace que el archivo sea más largo cada vez que se repite el comando. Si el archivo no existe cuando intenta agregar el resultado redirigido, se creará el archivo.

Entrada estándar
Muchos comandos pueden aceptar entradas de una instalación llamada entrada estándar . Por defecto, la entrada estándar obtiene su contenido del teclado, pero al igual que la salida estándar, se puede redirigir. Para redirigir la entrada estándar desde un archivo en lugar del teclado, el carácter "<" se usa así:

[me @ linuxbox me] $ sort <file_list.txt

En el ejemplo anterior, utilizamos el comando de clasificación para procesar el contenido de file_list.txt. Los resultados se muestran en la pantalla ya que la salida estándar no se redirigió. Podríamos redirigir la salida estándar a otro archivo como este:

[me @ linuxbox me] $ sort <file_list.txt> sorted_file_list.txt

Como puede ver, un comando puede tener tanto su entrada como su salida redirigidas. Tenga en cuenta que el orden de la redirección no importa. El único requisito es que los operadores de redireccionamiento ("<" y ">") deben aparecer después de las otras opciones y argumentos en el comando.

Tuberías
Lo más útil y poderoso que puede hacer con la redirección de E / S es conectar varios comandos junto con lo que se llaman tuberías . Con las tuberías, la salida estándar de un comando se alimenta a la entrada estándar de otro. Aquí está mi favorito absoluto:

[yo @ linuxbox me] $ ls -l | Menos

En este ejemplo, la salida del comando ls se introduce en less . Al usar este truco "| less" , puede hacer que cualquier comando tenga una salida de desplazamiento. Yo uso esta técnica todo el tiempo.

Al conectar comandos juntos, puedes lograr hazañas increíbles. Aquí hay algunos ejemplos que querrás probar:

Ejemplos de comandos utilizados junto con tuberías
Mando	Que hace
ls -lt | cabeza

Muestra los 10 archivos más nuevos en el directorio actual.

du | sort -nr

Muestra una lista de directorios y cuánto espacio consumen, ordenados de mayor a menor.

encontrar . -tipo f -print | wc -l

Muestra el número total de archivos en el directorio de trabajo actual y todos sus subdirectorios.

Filtros
Un tipo de programa utilizado con frecuencia en las tuberías se llama filtros . Los filtros toman la entrada estándar y realizan una operación sobre ella y envían los resultados a la salida estándar. De esta manera, se pueden combinar para procesar información de manera poderosa. Estos son algunos de los programas comunes que pueden actuar como filtros:

Comandos de filtro comunes
Programa	Que hace
ordenar

Ordena la entrada estándar y luego genera el resultado ordenado en la salida estándar.

uniq

Dada una secuencia ordenada de datos desde la entrada estándar, elimina líneas de datos duplicadas (es decir, se asegura de que cada línea sea única).

grep

Examina cada línea de datos que recibe de la entrada estándar y emite cada línea que contiene un patrón específico de caracteres.

fmt

Lee el texto de la entrada estándar, luego emite texto formateado en la salida estándar.

pr

Toma la entrada de texto de la entrada estándar y divide los datos en páginas con saltos de página, encabezados y pies de página en preparación para la impresión.

cabeza

Emite las primeras líneas de su entrada. Útil para obtener el encabezado de un archivo.

cola

Emite las últimas líneas de su entrada. Útil para cosas como obtener las entradas más recientes de un archivo de registro.

tr

Traduce personajes. Se puede usar para realizar tareas como conversiones en mayúsculas / minúsculas o cambiar los caracteres de terminación de línea de un tipo a otro (por ejemplo, convertir archivos de texto de DOS en archivos de texto de estilo Unix).

sed

Editor de transmisiones. Puede realizar traducciones de texto más sofisticadas que tr .

awk

Un lenguaje de programación completo diseñado para construir filtros. Extremadamente poderoso.


Realizar tareas con tuberías
Impresión desde la línea de comando. Linux proporciona un programa llamado lpr que acepta entradas estándar y las envía a la impresora. A menudo se usa con tuberías y filtros. Aquí hay un par de ejemplos:

gato pobremente_formateado_reporte.txt | fmt | pr | lpr

gato unsorted_list_with_dupes.txt | ordenar | uniq | pr | lpr

En el primer ejemplo, usamos cat para leer el archivo y enviarlo a la salida estándar, que se canaliza a la entrada estándar de fmt. fmt formatea el texto en párrafos claros y lo envía a la salida estándar, que se canaliza a la entrada estándar de pr. pr divide el texto ordenadamente en páginas y lo envía a la salida estándar, que se canaliza a la entrada estándar de lpr. lpr toma su entrada estándar y la envía a la impresora.

El segundo ejemplo comienza con una lista sin clasificar de datos con entradas duplicadas. Primero, cat envía la lista en orden que la clasifica y la alimenta en uniq que elimina cualquier duplicado. Siguiente PR y LPR se utilizan para paginate e imprimir la lista.

Ver el contenido de los archivos tar A menudo verá software distribuido como un archivo tar comprimido . Este es un archivo de cinta de estilo tradicional de Unix (creado con tar ) que se ha comprimido con gzip . Puede reconocer estos archivos por sus extensiones de archivo tradicionales, ".tar.gz" o ".tgz". Puede usar el siguiente comando para ver el directorio de dicho archivo en un sistema Linux:
tar tzvf name_of_file.tar.gz | Menos


