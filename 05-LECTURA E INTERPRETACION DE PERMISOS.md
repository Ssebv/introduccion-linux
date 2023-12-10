echo "hola hola" > file.txt
echo "hola 2" >> file.txt

- Los permisos en Linux controlan las acciones que se pueden realizar en archivos y directorios.
- Los permisos se aplican a tres niveles: propietario, grupo y otros.
- Se utilizan códigos numéricos para representar los permisos: r (lectura) = 4, w (escritura) = 2, x (ejecución) = 1.
- El comando `chmod` se utiliza para cambiar los permisos de un archivo o directorio.
- Ejemplo de uso de `chmod`: `chmod 755 archivo` (establece permisos de lectura, escritura y ejecución para el propietario, y solo permisos de lectura y ejecución para grupo y otros).
- El comando `ls -l` muestra los permisos de los archivos y directorios en formato detallado.


##### Grupos:

- En Linux, los grupos son una forma de organizar y gestionar los permisos de acceso a archivos y directorios.
- Cada usuario en Linux pertenece a uno o más grupos.
- Los grupos permiten compartir archivos y recursos entre los usuarios que pertenecen al mismo grupo.
- Los permisos de grupo se aplican a los usuarios que pertenecen al grupo asociado con un archivo o directorio.
- Al establecer permisos de grupo en un archivo o directorio, los usuarios que pertenecen al grupo pueden tener acceso y realizar acciones específicas según los permisos otorgados.

##### Otros:

- El término "otros" en Linux se refiere a todos los usuarios que no son el propietario del archivo ni pertenecen al grupo asociado.
- Los permisos para "otros" se aplican a cualquier usuario que no sea el propietario ni pertenezca al grupo del archivo o directorio.
- Los permisos "otros" permiten controlar qué acciones pueden realizar los usuarios no autorizados o no relacionados con el archivo o directorio.

- Permisos y derechos en Linux: [https://blog.desdelinux.net/permisos-y-derechos-en-linux/?msclkid=22f8cb88ba8111ecb5d8a3db91f066ab](https://blog.desdelinux.net/permisos-y-derechos-en-linux/?msclkid=22f8cb88ba8111ecb5d8a3db91f066ab)
- Permisos básicos en Linux: [https://www.profesionalreview.com/2017/01/28/permisos-basicos-linux-ubuntu-chmod/](https://www.profesionalreview.com/2017/01/28/permisos-basicos-linux-ubuntu-chmod/)
- Permisos en Linux | Cómo son y cómo se cambian: [https://www.softzone.es/programas/linux/permisos-archivos-directorios-linux/](https://www.softzone.es/programas/linux/permisos-archivos-directorios-linux/)
- Cambiar permisos con comandos: [https://www.hostinger.es/tutoriales/cambiar-permisos-y-propietarios-linux-linea-de-comandos/](https://www.hostinger.es/tutoriales/cambiar-permisos-y-propietarios-linux-linea-de-comandos/)

## Cambiar los propietarios de archivos y carpetas

Para cambiar el propietario de un archivo y una carpeta, utilizaremos el comando **chown**. Esta es la sintaxis básica:

`chown [propietario/grupo propietario] [nombre del archivo]`

Digamos que tenemos un archivo llamado «**miarchivo.txt**«. Si queremos que el **propietario** del archivo sea «**hostinger**«, podemos utilizar este comando:

`chown hostinger miarchivo.txt`

Sin embargo, si queremos cambiar el **grupo propietario** del archivo a «**clientes**«, introduciremos esta línea en su lugar:

`chown :clientes miarchivo.txt`

Observa que usamos dos puntos **(:)** antes de «clientes» para indicar que se trata de un grupo propietario.

Ahora, para cambiar el propietario y el grupo al mismo tiempo, la sintaxis sería así:

`chown hostinger:clientes miarchivo.txt`

Con chgrp cambiamos el grupo del usuario ya que cada usuario pertenece a un grupo con su mismo nombre

`chgrp [nombre grupo] [archivo]`

Con id podemos ver a que grupos pertenecemos

La regla principal es que el propietario debe ir antes del grupo propietario, y tienen que estar separados por dos puntos.

## Usar opciones con los comandos chmod y chown

Una **opción** es un comando adicional para modificar la respuesta de un comando.

Una de las opciones más populares que puedes combinar con **chmod** y **chown** es **-R** (recursivo). Esta opción te permite cambiar los permisos o propietarios de todos los archivos y subdirectorios dentro de un directorio específico.

Si quieres utilizar una opción, tienes que colocarla justo después del comando **chmod/chown**.

`chown -R 755 /etc/misarchivos`

Después de introducir el comando anterior, el propietario puede leer, escribir y ejecutar todos los archivos y subdirectorios dentro del directorio **/etc/misarchivos**. El comando también da permisos de lectura y ejecución al grupo y a otros.

- Asignación de permisos: [https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/](https://www.ionos.es/digitalguide/servidores/know-how/asignacion-de-permisos-de-acceso-con-chmod/)
- Propietarios y permisos: [https://atareao.es/tutorial/terminal/propietarios-y-permisos/](https://atareao.es/tutorial/terminal/propietarios-y-permisos/)

## Creacion de un nuevo usuario

- `useradd`: Comando para crear un nuevo usuario en el sistema.
    
    - Opciones:
        - `-s`: Especifica el tipo de shell que se asignará al nuevo usuario.
        - `-d`: Indica el directorio principal (home) del nuevo usuario.
- `passwd`: Comando para asignar una contraseña al usuario recién creado.
    
- `chgrp`: Comando para cambiar el grupo propietario de un archivo o directorio.
    
- `chown`: Comando para cambiar el propietario de un archivo o directorio.
    
    - Ejemplo: `chown ressc:root pepe` asigna el usuario "ressc" como propietario y el grupo "root" al archivo "pepe".
- `su`: Comando para cambiar al usuario especificado y acceder a su entorno de trabajo.
    
- `groupadd`: Comando para crear un nuevo grupo en el sistema.
    
- `cat /etc/group | grep [nombre del grupo]`: Comando para identificar el identificador del grupo y verificar si hay usuarios dentro de ese grupo específico.
    
- `usermod -a -G [nombre del grupo] [usuario]`: Comando para añadir un usuario existente a un grupo específico.
- `userdel -r nombre_usuario`: Elimina recursivamente todo lo del usuario
- `groupdel nombre_grupo`: Elimina el grupo creado

- Gestión de usuarios, grupos y permisos en Linux: [https://computernewage.com/2016/05/22/gestionar-usuarios-y-permisos-en-linux/](https://computernewage.com/2016/05/22/gestionar-usuarios-y-permisos-en-linux/)
- Gestión de usuarios y grupos en Linux: [https://atareao.es/como/gestion-de-usuarios-y-grupos-en-linux/](https://atareao.es/como/gestion-de-usuarios-y-grupos-en-linux/)