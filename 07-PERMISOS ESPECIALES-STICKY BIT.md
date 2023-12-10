
- `chmod +t [directorio]`: Este comando se utiliza para establecer el sticky bit en un directorio en Linux.
- El sticky bit es un permiso especial que se puede asignar a un directorio en sistemas basados en Unix.
- Cuando el sticky bit está habilitado en un directorio, solo el propietario del archivo o el root pueden eliminar o renombrar archivos dentro de ese directorio, incluso si otros usuarios tienen permisos de escritura en el directorio.
- El sticky bit se representa visualmente en la salida del comando `ls -l` con una "t" minúscula en lugar del bit de ejecución "x" para los permisos de otros (others) en el directorio.
- Este permiso se utiliza principalmente en directorios compartidos, como /tmp, para evitar que los usuarios eliminen o modifiquen archivos pertenecientes a otros usuarios.
- La opción "+t" en `chmod` se utiliza para agregar el sticky bit a un directorio existente. Por ejemplo, `chmod +t /tmp` establecería el sticky bit en el directorio /tmp.
- Para eliminar el sticky bit de un directorio, se puede utilizar la opción "-t" en `chmod`. Por ejemplo, `chmod -t /tmp` eliminaría el sticky bit del directorio /tmp.

##### ANEXO

¿Qué es el Sticky Bit y cómo configurarlo?: [https://keepcoding.io/blog/que-es-el-sticky-bit-y-como-configurarlo/](https://keepcoding.io/blog/que-es-el-sticky-bit-y-como-configurarlo/)
El bit Sticky | Tutorial de GNU/Linux: [https://www.fpgenred.es/GNU-Linux/el_bit_sticky.html](https://www.fpgenred.es/GNU-Linux/el_bit_sticky.html)b