
- La notación octal se utiliza para representar los permisos de lectura, escritura y ejecución en Linux.
- Los permisos se representan mediante un número de 3 dígitos en base octal (0 al 7).
- Cada dígito en el número octal representa los permisos para un nivel diferente: propietario, grupo y otros.
- Cada dígito se traduce a su equivalente binario de 3 bits, donde cada bit representa un permiso (r, w, x).
- El primer dígito representa los permisos del propietario, el segundo dígito representa los permisos del grupo, y el tercer dígito representa los permisos de otros.
- Cada permiso se asigna un valor en la notación octal: r (lectura) = 4, w (escritura) = 2, x (ejecución) = 1.
- Para asignar permisos, se suma los valores correspondientes. Por ejemplo, rwx sería 4 + 2 + 1 = 7 (permisos completos), rw- sería 4 + 2 = 6 (lectura y escritura).
- El número octal se coloca como parte del comando `chmod` para establecer los permisos. Por ejemplo, `chmod 755 archivo` establece permisos de lectura, escritura y ejecución para el propietario, y solo permisos de lectura y ejecución para grupo y otros.

- Permisos del sistema de archivos GNU/Linux: [https://blog.alcancelibre.org/staticpages/index.php/permisos-sistema-de-archivos](https://blog.alcancelibre.org/staticpages/index.php/permisos-sistema-de-archivos)
- Los permisos de UNIX, Linux y Mac OS: [https://www.prored.es/los-permisos-de-unix-linux-y-mac-os/](https://www.prored.es/los-permisos-de-unix-linux-y-mac-os/)