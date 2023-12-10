

```bash
touch script.sh
```
- Crea un nuevo archivo llamado "script.sh". En realidad no es necesario darle si se ejecuta con bash [nombre del archivo]

```bash
chmod +x script.sh
```
- Otorga permisos de ejecución al archivo "script.sh". 

```bash
#!/bin/bash
```
- Esta línea es el shebang y especifica el intérprete a utilizar para ejecutar el script (en este caso, Bash).

```bash
echo "Hola esto es una prueba"
```
- Imprime en la salida estándar el mensaje "Hola esto es una prueba".

```bash
ip a | grep ens33 | tail -n 1
```
- Muestra la información relacionada con la interfaz de red "ens33" utilizando los comandos `ip a` (muestra información de todas las interfaces de red), `grep` (filtra las líneas que contienen "ens33") y `tail -n 1` (muestra solo la última línea).

```bash
ip a | grep ens33 | awk 'NR == 2'
```
- Muestra la segunda línea de la salida que coincide con "ens33" utilizando los comandos `ip a`, `grep` y `awk`.

```bash
ip a | grep ens33 | tail -n 1 | awk '{print $2}'
```
- Muestra el segundo campo de la última línea de la salida que coincide con "ens33" utilizando los comandos `ip a`, `grep`, `tail -n 1` y `awk`.

```bash
ip a | grep ens33 | tail -n 1 | awk '{print $2}' | awk '{print $1}' FS="/"
```
- Muestra la primera parte del segundo campo de la última línea de la salida que coincide con "ens33" al dividirlo utilizando "/" como separador. Se utilizan dos comandos `awk` para lograr esto.

```bash
ip a | grep ens33 | tail -n 1 | awk '{print $2}' | cut -d '/' -f 1
```
- Muestra la primera parte del segundo campo de la última línea de la salida que coincide con "ens33" al dividirlo utilizando "/" como delimitador utilizando el comando `cut`.

```bash
ip a | grep ens33 | tail -n 1 | awk '{print $2}' | tr '/' ' ' | awk '{print $1}'
```
- Reemplaza las barras "/" por espacios en el segundo campo de la última línea de la salida que coincide con "ens33" utilizando el comando `tr`, y luego muestra la primera parte de este campo utilizando `awk`.

