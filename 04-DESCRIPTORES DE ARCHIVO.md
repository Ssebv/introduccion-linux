[[exec]] -> Link
[[file]] -> Link

- `exec 3<> file`: Abre el archivo "file" con lectura y escritura simultáneas a través del descriptor de archivo 3.
- `exec 3> file`: Abre el archivo "file" solo en modo de escritura a través del descriptor de archivo 3.
- `read <&3`: Lee el contenido del archivo asociado al descriptor de archivo 3.
- `echo "Nuevo contenido" >&3`: Escribe "Nuevo contenido" en el archivo asociado al descriptor de archivo 3.
- `file file`: Detecta el tipo de archivo basándose en los primeros bytes del archivo llamado "file".
- `exec 3>&-`: Cierro el descriptor de archivo pero no se elimina su contenido, eso significa que no podremos mandar el contenido al descriptor del archivo
- `exec 8>&5`:  Crea una copia del descriptor de archivo 5 en el 8 eso significa que todo lo que escriba en el 5 se escribira en el 8
- `exec 8>&5-`: Con un menos al final significa que cerramos el descriptor de archivo al que estamos copiando eso significa que solo quedaria el 8

Apoyo para profundizar sobre el uso de descriptores de archivo en Bash:

-  Redirectores en Bash [Formato PDF]: [https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf](https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf)

  
- `cmd >> file`: Ejecuta el comando `cmd` y redirige la salida al final del archivo especificado por `file`. Si el archivo no existe, se crea; si ya existe, se añade contenido al final sin sobrescribir el archivo existente.
- `cmd 2>> file`: Ejecuta el comando `cmd` y redirige la salida de error estándar (stderr) al final del archivo especificado por `file`. Al igual que en el caso anterior, si el archivo no existe, se crea; si ya existe, se añade contenido al final sin sobrescribir el archivo existente.
- `cmd > file 2>&1`: Ejecuta el comando `cmd` y redirige tanto la salida estándar (stdout) como la salida de error estándar (stderr) al archivo especificado por `file`. Esto significa que tanto la salida normal como los mensajes de error se redirigen al archivo.
- `cmd < file`: Ejecuta el comando `cmd` y redirige la entrada estándar (stdin) desde el archivo especificado por `file`. Esto permite que el comando lea la entrada desde el contenido del archivo en lugar del teclado.
- `cmd <<< "String"`: Ejecuta el comando `cmd` y le pasa una cadena de texto entre comillas como entrada. El texto se proporciona como una línea completa de entrada al comando.
- `cmd <(cmd1)`: Ejecuta el comando `cmd1` y redirige su salida a través de un archivo temporal en memoria que es pasado como argumento a `cmd`. Esto se conoce como proceso sustituto o proceso subshell.
- `cmd < <(cmd1)`: Similar al caso anterior, pero redirige la salida de `cmd1` como entrada al comando `cmd`.
- `cmd <(cmd1) <(cmd2)`: Ejecuta los comandos `cmd1` y `cmd2` y redirige su salida a través de archivos temporales en memoria. Ambos archivos se pasan como argumentos al comando `cmd`.
- `cmd1 >(cmd2)`: Ejecuta los comandos `cmd1` y `cmd2` y redirige la salida estándar de `cmd1` como entrada a `cmd2`. Esto permite que `cmd2` reciba la salida de `cmd1` como si fuera un archivo.
- `cmd1 > >(cmd2)`: Similar al caso anterior, pero redirige la salida estándar de `cmd1` como entrada al comando `cmd2`. Esto permite que `cmd2` reciba la salida de `cmd1` como si fuera un archivo.
- `cmd1 | cmd2`: Ejecuta `cmd1` y redirige su salida estándar como entrada al comando `cmd2`. Esto permite que `cmd2` procese la salida de `cmd1`.
- `cmd1 |& cmd2`: Ejecuta `cmd1` y redirige tanto su salida estándar como la salida de error estándar al comando `cmd2`. Esto permite que `cmd2` procese tanto la salida normal como los mensajes de error de `cmd1`.
- `cmd | tee file`: Ejecuta `cmd` y muestra su salida tanto en la pantalla como la redirige al archivo especificado por `file`. La utilidad `tee` es especialmente útil cuando se desea visualizar la salida de un comando en tiempo real mientras se guarda en un archivo.