Chuleta de comandos Linux: [https://ciberninjas.com/chuleta-comandos-linux/](https://ciberninjas.com/chuleta-comandos-linux/)
Comandos básicos de Linux: [https://www.bonaval.com/kb/cheats-chuletas/comandos-basicos-linux](https://www.bonaval.com/kb/cheats-chuletas/comandos-basicos-linux)
Guía en PDF de comandos básicos de Linux: [https://www.fing.edu.uy/inco/cursos/sistoper/recursosLaboratorio/tutorial0.pdf](https://www.fing.edu.uy/inco/cursos/sistoper/recursosLaboratorio/tutorial0.pdf)

Cada terminal que se abre en un sistema Unix se asigna a un usuario específico. Podemos identificar con qué usuario estamos ingresados utilizando varios comandos:

- `whoami`: Este comando muestra el nombre del usuario que inició la sesión actual.
    
- `id`: Nos permite ver el usuario actual y los grupos a los que pertenece.
    
- `exit`: Nos permite salir de la terminal.
    

Observamos que pertenecemos a varios grupos y que tenemos la capacidad de escalar a usuario root, ya que somos parte del grupo sudo. También cabe destacar que no necesariamente necesitamos cambiar a root para ejecutar comandos con privilegios de root.

Existen dos tipos de rutas en los sistemas Unix:

- Rutas absolutas: Son rutas que comienzan desde la raíz del sistema. Por ejemplo, `/usr/bin/whoami` es una ruta absoluta. Podemos obtener la ruta absoluta de un comando con `which <comando>`, por ejemplo, `which whoami`.
    
- Rutas relativas: Son rutas que se especifican relativas al directorio actual.
    

El comando `echo` es similar a la función print en varios lenguajes de programación. Se utiliza para imprimir información en la terminal. Por ejemplo, `echo $PATH` imprime el valor de la variable de entorno `PATH`.

Los comandos pueden recibir entradas de datos a través de pipes. Por ejemplo:

`cat /etc/group | grep "floppy"`
`cat /etc/group | grep "floppy" -n # Esto también muestra el número de línea`

Existen varias formas de lograr la misma tarea en la terminal. Por ejemplo, `command -v whoami` hace lo mismo que `which whoami`.

Algunos otros comandos útiles son:

- `pwd`: Muestra el directorio de trabajo actual.
    
- `ls`: Lista el contenido del directorio actual. Con las opciones `-l` y `/bin/`, por ejemplo, se puede listar el contenido de un directorio con detalle y desde cualquier lugar.
    
- `cd`: Cambia el directorio de trabajo.
    

En el sistema Unix, el comando `cd /` nos lleva a la raíz del sistema operativo, y `cd` sin argumentos nos lleva al directorio principal del usuario actual.

Podemos obtener información adicional sobre el usuario actual con los siguientes comandos:

- `echo $HOME`: Muestra el directorio principal del usuario.
    
- `echo $SHELL`: Muestra el shell actual del usuario.
    
- `cat /etc/passwd`: Muestra la lista de todos los usuarios del sistema.
    
- `cat /etc/shells`: Muestra los diferentes tipos de shells disponibles en el sistema.
    
- `mkdir <nombre_del_directorio>`: Este comando crea un nuevo directorio con el nombre proporcionado.
    
- `touch <nombre_del_archivo>`: Este comando crea un nuevo archivo vacío con el nombre proporcionado.
    
- `rm <nombre_del_archivo>`: Este comando elimina un archivo. Si deseas eliminar un directorio y su contenido, puedes usar `rm -r <nombre_del_directorio>`.
    
- `mv <nombre_del_archivo> <nuevo_lugar>`: Este comando mueve un archivo o directorio a una nueva ubicación.
    
- `cp <nombre_del_archivo> <nuevo_lugar>`: Este comando copia un archivo o directorio a una nueva ubicación.
    

El manejo de permisos es una parte crucial de los sistemas Unix. Los permisos determinan quién puede leer, escribir o ejecutar un archivo o directorio. Aquí hay algunos comandos relacionados con los permisos:
![[PermisosUnix.png]]
- `chmod <permisos> <nombre_del_archivo>`: Este comando cambia los permisos de un archivo o directorio. Los permisos se pueden especificar como un número octal (por ejemplo, 755) o como una cadena simbólica (por ejemplo, u+x).
    
- `chown <nuevo_propietario> <nombre_del_archivo>`: Este comando cambia el propietario de un archivo o directorio.
    
- `chgrp <nuevo_grupo> <nombre_del_archivo>`: Este comando cambia el grupo de un archivo o directorio.
    

En un sistema Unix, también puedes trabajar con procesos:

- `ps`: Este comando muestra los procesos actuales.
    
- `top`: Este comando muestra una vista en tiempo real de los procesos que se están ejecutando.
    
- `kill <ID_del_proceso>`: Este comando termina un proceso.
    

Finalmente, hay algunas otras características útiles que puedes utilizar en la terminal de Unix:

- Redirección de salida: Puedes redirigir la salida de un comando a un archivo usando `>`. Por ejemplo, `echo "Hello, world!" > hello.txt` guarda la cadena "Hello, world!" en el archivo `hello.txt`.
    
- Redirección de entrada: Puedes redirigir el contenido de un archivo a un comando usando `<`. Por ejemplo, `sort < unsorted.txt` ordenaría las líneas del archivo `unsorted.txt`.
    
- Piping: Puedes canalizar la salida de un comando a otro usando `|`. Por ejemplo, `cat /etc/passwd | grep "root"` buscaría la línea que contiene "root" en el archivo `/etc/passwd`.