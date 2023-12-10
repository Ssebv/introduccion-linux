

- `id`: Muestra información sobre el usuario actual, como su ID de usuario (UID) y los grupos a los que pertenece.
- `find / -name passwd 2> /dev/null`: Busca el archivo llamado "passwd" en todo el sistema de archivos y redirige los mensajes de error a /dev/null para evitar mostrar mensajes de permisos denegados.
- `find / -perm -4000 2> /dev/null`: Busca archivos con permisos establecidos como setuid, que permiten la ejecución con los privilegios del propietario, y redirige los mensajes de error a /dev/null.
- `find / -group [user] 2> /dev/null`: Busca archivos y directorios pertenecientes al grupo especificado, y redirige los mensajes de error a /dev/null.
- `find / -group [user] -type f 2> /dev/null` y `find / -group [user] -type d 2> /dev/null`: Busca archivos y directorios pertenecientes al grupo especificado, filtrando por tipo de archivo (archivo regular o directorio), y redirige los mensajes de error a /dev/null.
- `find / -user root -writable 2> /dev/null` y `find / -user root -executable -type f 2> /dev/null`: Busca archivos y directorios que pertenecen al usuario "root" y tienen permisos de escritura o son archivos ejecutables, y redirige los mensajes de error a /dev/null.
- `find / -name dex \\ * 2> /dev/null` y `find / -name \\* dex\\* .sh 2> /dev/null`: Busca archivos y directorios con nombres que contienen la palabra "dex" o tienen extensión ".sh", y redirige los mensajes de error a /dev/null.
- `find --help`: Muestra información de ayuda sobre el comando `find`.
- `find / -name \\* dex\\* .sh -ls 2> /dev/null`: Busca archivos con nombres que contienen la palabra "dex" o tienen extensión ".sh" y muestra información detallada (`-ls`) de los archivos encontrados, redirigiendo los mensajes de error a /dev/null.

Comandos Find y Locate en Linux: [https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/](https://www.hostinger.es/tutoriales/como-usar-comando-find-locate-en-linux/)

