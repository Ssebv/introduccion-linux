
- `xargs`: Se utiliza para paralelizar comandos o sentencias de comando. Toma la entrada de un comando anterior y la pasa como argumentos a otro comando.
    
    - Ejemplo: `which python3 | xargs ls -l` muestra información detallada (`ls -l`) del archivo binario `python3` encontrado por el comando `which`.
- `pkexec`: Es un comando utilizado para ejecutar programas como un superusuario. Sin embargo, se ha encontrado una vulnerabilidad en este comando, por lo que se recomienda eliminar los permisos de ejecución del bit "setuid" (u-s) en su archivo binario para mitigar el riesgo.
    
- `find`: Se utiliza para buscar archivos y directorios en el sistema de archivos.
    
    - Ejemplo: `find / -type f -perm -4000` busca archivos (`-type f`) con permisos establecidos como setuid (`-perm -4000`) en todo el sistema.
- `chmod g+s [ruta]`: Establece el bit "setgid" en un archivo o directorio, lo que indica que los nuevos archivos y directorios creados dentro de ese directorio heredarán el grupo del directorio padre.
    
- `chmod u+s [ruta]`: Establece el bit "setuid" en un archivo ejecutable, lo que permite que el programa se ejecute con los privilegios del propietario del archivo.

![[Ejemplo SUID.png]]

##### Uso de permisos SUID/SGID en los siguientes enlaces:

- Permisos SGID, SUID y Sticky Bit: [Permisos SGID, SUID y Sticky Bit](https://deephacking.tech/permisos-sgid-suid-y-sticky-bit-linux/#:~:text=Permiso%20SGID,-El%20permiso%20SGID&text=Si%20se%20establece%20en%20un,perteneciente%2C%20el%20grupo%20del%20directorio.)
- Permisos especiales en Linux: [Permisos especiales en Linux – Sticky Bit, SUID y SGID](https://www.ochobitshacenunbyte.com/2019/06/17/permisos-especiales-en-linux-sticky-bit-suid-y-sgid/)
- Los bits SUID, SGID y Sticky: [Los bits SUID, SGID y Sticky](https://www.ibiblio.org/pub/linux/docs/LuCaS/Manuales-LuCAS/SEGUNIX/unixsec-2.1-html/node56.html)