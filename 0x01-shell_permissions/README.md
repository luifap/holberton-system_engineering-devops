
Gráfico del título
Casa
Aprendiendo la cáscara
Escribir guiones de Shell
Recursos
El libro
Aventuras
Blog
Anterior | Contenidos | próximo

Permisos
Los sistemas operativos tipo Unix, como Linux, difieren de otros sistemas informáticos en que no solo son multitarea, sino también multiusuario .

¿Qué significa esto exactamente? Significa que más de un usuario puede operar la computadora al mismo tiempo. Si bien su computadora solo tiene un teclado y un monitor, aún puede ser utilizada por más de un usuario. Por ejemplo, si su computadora está conectada a una red o a Internet, los usuarios remotos pueden iniciar sesión a través de ssh (shell seguro) y operar la computadora. De hecho, los usuarios remotos pueden ejecutar aplicaciones gráficas y mostrar la salida en una computadora remota. El sistema X Window lo admite.

La capacidad multiusuario de los sistemas tipo Unix es una característica que está profundamente arraigada en el diseño del sistema operativo. Si recuerda el entorno en el que se creó Unix, esto tiene mucho sentido. Hace años, antes de que las computadoras fueran "personales", eran grandes, caras y centralizadas. Un sistema informático universitario típico consistía en una gran computadora central ubicada en algún edificio del campus y terminales ubicados en todo el campus, cada uno conectado a la gran computadora central. La computadora admitiría a muchos usuarios al mismo tiempo.

Para que esto sea práctico, se tuvo que idear un método para proteger a los usuarios unos de otros. Después de todo, no puede permitir que las acciones de un usuario bloqueen la computadora, ni puede permitir que un usuario interfiera con los archivos que pertenecen a otro usuario.

Esta lección cubrirá los siguientes comandos:

chmod - modificar derechos de acceso a archivos
su - conviértete temporalmente en superusuario
sudo : conviértete temporalmente en superusuario
chown - cambiar la propiedad del archivo
chgrp - cambia la propiedad del grupo de un archivo
Permisos de archivo
En un sistema Linux, a cada archivo y directorio se le asignan derechos de acceso para el propietario del archivo, los miembros de un grupo de usuarios relacionados y todos los demás. Se pueden asignar derechos para leer un archivo, escribir un archivo y ejecutar un archivo (es decir, ejecutar el archivo como un programa).

Para ver la configuración de permisos para un archivo, podemos usar el comando ls . Como ejemplo, veremos el programa bash que se encuentra en el directorio / bin :

[me @ linuxbox me] $ ls -l / bin / bash


-rwxr-xr-x 1 raíz raíz 316848 27 de febrero de 2000 / bin / bash
Aquí podemos ver:

El archivo "/ bin / bash" es propiedad del usuario "root"
El superusuario tiene derecho a leer, escribir y ejecutar este archivo
El archivo es propiedad del grupo "root"
Los miembros del grupo "root" también pueden leer y ejecutar este archivo
Todos los demás pueden leer y ejecutar este archivo
En el siguiente diagrama, vemos cómo se interpreta la primera parte de la lista. Consiste en un carácter que indica el tipo de archivo, seguido de tres conjuntos de tres caracteres que transmiten el permiso de lectura, escritura y ejecución para el propietario, el grupo y todos los demás.

diagrama de permisos

chmod
El comando chmod se usa para cambiar los permisos de un archivo o directorio. Para usarlo, especifique la configuración de permisos deseada y el archivo o archivos que desea modificar. Hay dos formas de especificar los permisos. En esta lección nos centraremos en uno de estos, llamado método de notación octal .

Es fácil pensar en la configuración de permisos como una serie de bits (que es como la computadora piensa acerca de ellos). Así es como funciona:

rwx rwx rwx = 111 111 111
rw- rw- rw- = 110110110
rwx --- --- = 111 000 000

y así...

rwx = 111 en binario = 7
rw- = 110 en binario = 6
rx = 101 en binario = 5
r-- = 100 en binario = 4

Ahora, si representa cada uno de los tres conjuntos de permisos (propietario, grupo y otros) como un solo dígito, tiene una forma bastante conveniente de expresar las posibles configuraciones de permisos. Por ejemplo, si quisiéramos configurar some_file para tener permiso de lectura y escritura para el propietario, pero quisiéramos mantener el archivo privado de otros, haríamos lo siguiente:

[me @ linuxbox me] $ chmod 600 some_file

Aquí hay una tabla de números que cubre todas las configuraciones comunes. Los que comienzan con "7" se usan con programas (ya que permiten la ejecución) y el resto son para otros tipos de archivos.

Valor	Sentido
777

(rwxrwxrwx) No hay restricciones en los permisos. Cualquiera puede hacer cualquier cosa. Generalmente no es un entorno deseable.

755

(rwxr-xr-x) El propietario del archivo puede leer, escribir y ejecutar el archivo. Todos los demás pueden leer y ejecutar el archivo. Esta configuración es común para los programas que utilizan todos los usuarios.

700

(rwx ------) El propietario del archivo puede leer, escribir y ejecutar el archivo. Nadie más tiene ningún derecho. Esta configuración es útil para programas que solo el propietario puede usar y deben mantenerse privados de otros.

666

(rw-rw-rw-) Todos los usuarios pueden leer y escribir el archivo.

644

(rw-r - r--) El propietario puede leer y escribir un archivo, mientras que todos los demás solo pueden leer el archivo. Una configuración común para los archivos de datos que todos pueden leer, pero solo el propietario puede cambiar.

600

(rw -------) El propietario puede leer y escribir un archivo. Todos los demás no tienen derechos. Una configuración común para archivos de datos que el propietario quiere mantener en privado.

Permisos de directorio
El comando chmod también se puede usar para controlar los permisos de acceso para directorios. Nuevamente, podemos usar la notación octal para establecer permisos, pero el significado de los atributos r, w y x es diferente:

r : permite que se enumere el contenido del directorio si también se establece el atributo x.
w : permite crear, eliminar o renombrar archivos dentro del directorio si también se establece el atributo x.
x : permite que se ingrese un directorio (es decir, cd dir ).
Aquí hay algunas configuraciones útiles para directorios:

Valor	Sentido
777

(rwxrwxrwx) No hay restricciones en los permisos. Cualquiera puede enumerar archivos, crear archivos nuevos en el directorio y eliminar archivos en el directorio. En general no es un buen escenario.

755

(rwxr-xr-x) El propietario del directorio tiene acceso completo. Todos los demás pueden enumerar el directorio, pero no pueden crear archivos ni eliminarlos. Esta configuración es común para los directorios que desea compartir con otros usuarios.

700

(rwx ------) El propietario del directorio tiene acceso completo. Nadie más tiene ningún derecho. Esta configuración es útil para directorios que solo el propietario puede usar y deben mantenerse privados de otros.

Convertirse en superusuario por un corto tiempo
A menudo es necesario convertirse en el superusuario para realizar tareas importantes de administración del sistema, pero como le han advertido, no debe permanecer conectado como superusuario. En la mayoría de las distribuciones, hay un programa que puede brindarle acceso temporal a los privilegios del superusuario. Este programa se llama su (abreviatura de usuario sustituto) y se puede usar en aquellos casos en los que necesita ser el superusuario para una pequeña cantidad de tareas. Para convertirse en el superusuario, simplemente escriba el comando su . Se le pedirá la contraseña del superusuario:

[me @ linuxbox me] $ su
Contraseña:
[root @ linuxbox me] #

Después de ejecutar el comando su , tiene una nueva sesión de shell como superusuario. Para salir de la sesión de superusuario, escriba exit y volverá a su sesión anterior.

En algunas distribuciones, especialmente Ubuntu, se utiliza un método alternativo. En lugar de usar su , estos sistemas emplean el comando sudo en su lugar. Con sudo , a uno o más usuarios se les otorgan privilegios de superusuario según sea necesario. Para ejecutar un comando como superusuario, el comando deseado simplemente está precedido por el comando sudo . Después de ingresar el comando, se le solicita al usuario su contraseña en lugar de la del superusuario:

[me @ linuxbox me] $ sudo some_command
Contraseña:
[me @ linuxbox me] $

Cambio de propiedad del archivo
Puede cambiar el propietario de un archivo utilizando el comando chown . Aquí hay un ejemplo: supongamos que quiero cambiar el propietario de some_file de "yo" a "usted". Yo podría:

[me @ linuxbox me] $ su
Contraseña:
[root @ linuxbox me] # chown you some_file
[root @ linuxbox me] # exit
[me @ linuxbox me] $
Tenga en cuenta que para cambiar el propietario de un archivo, debe ser el superusuario. Para hacer esto, nuestro ejemplo empleó el comando su , luego ejecutamos chown y finalmente escribimos exit para regresar a nuestra sesión anterior.

chown funciona de la misma manera en directorios que en archivos.

Cambio de propiedad del grupo
La propiedad del grupo de un archivo o directorio se puede cambiar con chgrp . Este comando se usa así:

[me @ linuxbox me] $ chgrp new_group some_file
En el ejemplo anterior, cambiamos la propiedad del grupo de some_file de su grupo anterior a "new_group". Debe ser el propietario del archivo o directorio para realizar un chgrp .

Anterior | Contenidos | Arriba | próximo

© 2000-2020, William E. Shotts, Jr. La copia literal y la distribución de este artículo completo están permitidas en cualquier medio, siempre que se conserve este aviso de copyright.

Linux® es una marca registrada de Linus Torvalds.

