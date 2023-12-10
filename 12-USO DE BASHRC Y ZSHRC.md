- `ls -a`: Muestra todos los archivos, incluidos los ocultos, en el directorio actual.
- `awk`: Es una herramienta de procesamiento de texto que permite realizar operaciones de búsqueda, filtrado y transformación en archivos de texto estructurados.
- `cut -d ' ' -f 1`: Extrae el primer campo de texto de una línea, utilizando el espacio como delimitador.
- `echo "Tu IP privada es: $(hostname -I | awk '{print $1}')"`: Es un comando utilizado para mostrar en pantalla la dirección IP privada del sistema. Utiliza la expansión de comandos `$()` para ejecutar el comando `hostname -I`, que obtiene la lista de direcciones IP del sistema, y luego utiliza `awk` para imprimir solo la primera dirección IP de la lista.
- `.bashrc` y `.zshrc`: Son archivos de configuración de los intérpretes de línea de comandos Bash y Zsh, respectivamente. Estos archivos permiten personalizar y configurar el entorno de la terminal, como definir variables de entorno, alias de comandos y funciones personalizadas.

Recordar que en parrot nuca debes de hacer sudo apt upgrade, parrot tiene su propio upgrade que esta hecho para parrot.

`parrot-upgrade`


En mi caso yo opero con una ZSH, por tanto mi archivo de configuración corresponde al ‘~/.**zshrc**‘. Recuerda que en caso de usar una BASH tu archivo de configuración debería estar situado en ‘**~/.bashrc**‘

Por aquí te dejo algunos enlaces de interés por si quieres entender un poco mejor para qué sirven estos archivos:

- ¿Qué es Bashrc en Linux?: [https://www.compuhoy.com/que-es-bashrc-en-linux/](https://www.compuhoy.com/que-es-bashrc-en-linux/)
- ¿Por qué deberías usar ZSH?: [https://respontodo.com/que-es-zsh-y-por-que-deberia-usarlo-en-lugar-de-bash/](https://respontodo.com/que-es-zsh-y-por-que-deberia-usarlo-en-lugar-de-bash/)

Mi recomendación personal es que utilicéis ZSH en vez de BASH, desde que investiguéis un poco diferencias entre ambas entenderéis por qué merece la pena.