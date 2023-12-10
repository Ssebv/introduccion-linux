-  Redirectores en Bash [Formato PDF]: [https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf](https://hack4u.io/wp-content/uploads/2022/05/bash-redirections-cheat-sheet.pdf)
- Todos los codigos de estado :  [https://datatracker.ietf.org/doc/html/rfc2616#page-62](https://datatracker.ietf.org/doc/html/rfc2616#page-62)
Cada proceso en un sistema Unix tiene un identificador único llamado PID (Process ID). Este identificador se utiliza para gestionar y controlar los procesos.

Los comandos en Unix pueden ser ejecutados de manera secuencial (uno después del otro) o condicionalmente. Aquí tienes algunos ejemplos:

- `whoami; ls`: Ejecuta `whoami` y luego `ls`, sin importar el resultado del primer comando.
    
- `whoami && ls`: Ejecuta `whoami`, y si éste es exitoso (devuelve un código de estado 0), entonces ejecuta `ls`.
    
- `whoami || ls`: Ejecuta `whoami`, y si éste falla (devuelve un código de estado distinto de 0), entonces ejecuta `ls`.
    

El comando `echo $?` muestra el código de estado del último comando ejecutado.

En Unix, la salida de error se llama stderr y la salida estándar se llama stdout.

El dispositivo `/dev/null` se comporta como un "agujero negro" en Unix. Cualquier dato que se le envíe desaparecerá.

El operador `>` se utiliza para redirigir la salida estándar o stderr a un archivo o dispositivo. Por ejemplo, `whoami > /dev/null 2>&1` redirige tanto la salida estándar como stderr a `/dev/null`.

La sintaxis `&>` es un atajo para redirigir tanto la salida estándar como stderr. Por ejemplo, `cat /etc/hosts &> /dev/null` envía tanto la salida estándar como stderr de `cat /etc/hosts` a `/dev/null`.

Al agregar `&` al final de un comando, el proceso se ejecuta en segundo plano. Por ejemplo, `wireshark &> /dev/null &` ejecuta Wireshark en segundo plano y redirige su salida estándar y stderr a `/dev/null`.

El comando `disown` se utiliza para quitar un proceso de la lista de procesos del shell actual. Esto significa que el proceso ya no se cerrará si cierras el shell. Por ejemplo, `wireshark &> /dev/null & disown` ejecuta Wireshark en segundo plano, redirige su salida estándar y stderr a `/dev/null`, y luego desvincula el proceso del shell actual. Esto te permite cerrar la terminal sin terminar Wireshark.