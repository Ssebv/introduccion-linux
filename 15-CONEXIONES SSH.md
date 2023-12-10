
OverTheWire: [https://overthewire.org/wargames/bandit/bandit0.html](https://overthewire.org/wargames/bandit/bandit0.html)

- SSH (Secure Shell) es un protocolo de red seguro para conexión y acceso remoto.
- Proporciona una forma segura de iniciar sesión y ejecutar comandos en dispositivos remotos.
- Utiliza cifrado para proteger la comunicación y garantizar la confidencialidad de los datos.
- Permite autenticación segura con diferentes métodos, como contraseña o clave pública/privada.
- Es ampliamente utilizado en administración de sistemas y redes para gestionar dispositivos remotos.
- Utiliza el puerto TCP 22 de forma predeterminada.
- Compatible con diferentes sistemas operativos y tiene implementaciones como OpenSSH y PuTTY.

---

### Informacion OverTheWire
- SSH Information
- Host: bandit.labs.overthewire.org
- Port: 2220
### Conexion
`ssh bandit0@bandit.labs.overthewire.org -p 2220` # Como contraseña es bandit0

- Para configurar el ctrl + l de limpiar la terminal es tan solo ingresando el siguiente comando
`export TERM=xterm`

---

#### Nivel 1
- En el archivo readme tengo la contraseña para conectarme a bandit1

---

#### Nivel 2
- Al ser un archivo llamado - no se le puede realizar un cat ya que cuenta con - de un parametro pero si realizamos un cat con la ruta absoluta funcinaria la perfeccion
`cat /home/bandit1/-` o `cat ./-`

`rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi`# Password bandit2

- Una forma de conectarnos directamente sin que nos pida la contraseña es entregandosela antes :
   `sshpass -p '[passwd]' ssh  bandit2@bandit.labs.overthewire.org -p 2220`

---

#### Nivel 3

- Cuando se quiera realizar un cat de un archivo con espacios perfectamente lo podemos envolver en comillas
   `cat "spaces in this filename"`
- Tambien podria ser con cat s* como si especificaramos que comienza con s 
   `aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG` # Password this level

---

#### Nivel 4

- Puedo listar los archivos ocultos de la siguiente manera
   `cat .hidden` 
- Puedo utilzar tambien el find de una carpeta anterior y reazar un grep para encotrar la linea que se decea y xargs para concatener el cat
   `find . | grep hidden | xargs cat`
- Se puede realizar tambien con grep la elminacion de las lineas deceadas 
   `find . -type f | grep -vE "bashr|prof|bash" | xargs cat`

`2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe`# Password

---

#### Nivel 5

La columna correspondiente a *‘Firma Hexadecimal‘* está directamente relacionada a los primeros bytes que en comparación a los primeros bytes del archivo con el que estamos tratando nos permitirán saber el tipo de formato al que nos estamos enfrentando:
- Lista de firmas de archivos: https://en.wikipedia.org/wiki/List_of_file_signatures

```bash
bandit4@bandit:~/inhere$ find . -type f | xargs file
./-file03: data
./-file06: data
./-file08: data
./-file07: ASCII text
./-file04: data
./-file00: data
./-file01: data
./-file02: data
./-file09: Non-ISO extended-ASCII text, with no line terminators
./-file05: data
```


```bash
bandit4@bandit:~/inhere$ find . -type f | grep 07 | xargs cat
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```

---

#### Nivel 6

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable
***Commands you may need to solve this level
ls , cd , cat , file , du , find***

```bash
bandit5@bandit:~/inhere$ find . -type f -readable ! -executable -size 1033c
```

```bash
bandit5@bandit:~/inhere$ find . -type f -readable ! -executable -size 1033c | xargs cat | head -n 1
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
bandit5@bandit:~/inhere$ find . -type f -readable ! -executable -size 1033c | xargs cat | xargs
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```

---

#### Nivel 7

The password for the next level is stored somewhere on the server and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

***Commands you may need to solve this level
ls , cd , cat , file , du , find , grep***

```bash
bandit6@bandit:/$ find . -size 33c -user bandit7 2> /dev/null | head -n 1
./var/lib/dpkg/info/bandit7.password
bandit6@bandit:/$ find . -size 33c -user bandit7 2> /dev/null | head -n 1 | xargs cat 
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```

---

#### Nivel 8

The password for the next level is stored in the file data.txt next to the word millionth

***Commands you may need to solve this level
man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd***

```bash
bandit7@bandit:~$ cat data.txt | grep millionth
millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP

bandit7@bandit:~$ cat data.txt | grep millionth | awk 'NF{print $NF}'
TESKZC0XvTetK0S9xNwm25STk5iWrBvP

bandit7@bandit:~$ cat data.txt | grep millionth | awk '{print $2}'
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```

```bash
seq 1 20 # Creo una secu en bash
for i in $(seq 1 20); do echo -e "\n[+] Probando con $i"; done # Bucle for
rev # Invierte la cadena
xargs # Si lo utilizas solo como que formateas la cadena entera mediante los espacios
tr ' ' '\n' # Transformamos los espacios en salto de linea # No se puede cambiar de palabras a palabra siempre debe de tener el mismo caracter de largo 
sed 's/prueba/probando' # asi se cambian las palabras en bash
sed 's/prueba/probando/g' # con la g al final le decimos que es para todas la palabras prueba del texto, de la misma forma funciona en vim
tail -n 1 # Seleccionamos la primera palabra de la linea
```

##### awk
- AWK Cheat Sheet: https://www.shortcutfoo.com/app/dojos/awk/cheatsheet
- AWK Cheat Sheet 2: https://bl831.als.lbl.gov/~gmeigs/scripting_help/awk_cheat_sheet.pdf
##### cut
- Cheat Sheet: Cutting Text with cut: https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/

---

#### Nivel 9

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

***Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd***

```bash
bandit8@bandit:~$ cat data.txt | sort | uniq -u 
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```

- El comando uniq -u permite identificar la unica linea que no se repite
- con sort ordenamos todo el archivo

*Helpful Reading Material
Piping and Redirection* : https://ryanstutorials.net/linuxtutorial/piping.php

Tutorial del uso de SORT: https://www.ibidemgroup.com/edu/tutorial-sort-linux-unix/
Tutorial del uso de UNIQ: https://victorhckinthefreeworld.com/2021/10/21/el-comando-uniq-de-gnu/

---
#### Nivel 10

The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

***Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd***

```bash
bandit9@bandit:~$ strings data.txt | grep "==="
4========== the#
========== password
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

- *head* -n [cuantas lineas superiores quiero]
- *tail* -n [cuantas lineas inferiores quiero]

```bash
bandit9@bandit:~$ strings data.txt | grep "===" | tail -n 1
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
bandit9@bandit:~$ strings data.txt | grep "===" | head -n 1
4========== the#
```

```bash
bandit9@bandit:~$ strings data.txt | grep "===" | tail -n 1 | awk '{print $2}'
G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

- Tambies se puede utilizar *NF* en *awk*  :  `awk 'NF{print $NF}'`

---
#### Nivel 11

The password for the next level is stored in the file data.txt, which contains base64 encoded data

***Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd***

##### Helpful Reading Material
[Base64 on Wikipedia](https://en.wikipedia.org/wiki/Base64)


**Base64** es un formato de codificación utilizado para convertir datos binarios en una representación de texto ASCII. Se utiliza comúnmente para transmitir o almacenar datos binarios en entornos que solo admiten texto, como el correo electrónico o los archivos de configuración. En Base64, cada grupo de 3 bytes de datos binarios se convierte en un grupo de 4 caracteres **ASCII**. Esta codificación permite que los datos binarios sean representados y manipulados como texto legible.

Algunos ejemplos comunes :

- *Base32*: Similar a Base64, pero utiliza un conjunto de caracteres diferentes para la representación de datos binarios. Es menos eficiente en términos de espacio, pero es útil en casos donde es necesario evitar caracteres especiales o confusos.

- *Hexadecimal (Hex)*: Utiliza una representación de caracteres hexadecimales (0-9 y A-F) para codificar datos binarios. Cada byte se convierte en dos caracteres hexadecimales.

- *ASCII*: El conjunto de caracteres ASCII estándar utiliza valores numéricos de 0 a 127 para representar caracteres. Cada carácter se representa con un byte.

- *UTF-8*: Es un formato de codificación de caracteres que puede representar cualquier carácter Unicode. Utiliza una variable cantidad de bytes para codificar caracteres, lo que permite la representación de caracteres de diferentes idiomas y símbolos especiales.


```bash
bandit10@bandit:~$ cat data.txt 
VGhlIHBhc3N3b3JkIGlzIDZ6UGV6aUxkUjJSS05kTllGTmI2blZDS3pwaGxYSEJNCg==
```

```bash
bandit10@bandit:~$ cat data.txt | base64 -d
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```
---

#### Nivel 12

The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
***Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd***

##### Helpful Reading Material
[Rot13 on Wikipedia](https://en.wikipedia.org/wiki/ROT13)

```bash
         "abcdefghijklmnopqrstuvwxyz"
```

```bash
bandit11@bandit:~$ cat data.txt | tr '[G-ZA-Fg-za-f]' '[T-ZA-St-za-s]'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
```bash
bandit11@bandit:~$ cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```

---
#### Nivel 13
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

***Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file***

##### Helpful Reading Material

[Hex dump on Wikipedia](https://en.wikipedia.org/wiki/Hex_dump)

```bash
bandit12@bandit:~$ cat data.txt 
00000000: 1f8b 0808 2773 4564 0203 6461 7461 322e  ....'sEd..data2.
00000010: 6269 6e00 0145 02ba fd42 5a68 3931 4159  bin..E...BZh91AY
00000020: 2653 597b 4f96 5f00 0018 ffff fd6f e7ed  &SY{O._......o..
00000030: bff7 bef7 9fdb d7ca ffbf edff 8ded dfd7  ................
00000040: bfe7 bbff bfdb fbff ffbf ff9f b001 3b56  ..............;V
00000050: 0400 0068 0064 3400 d341 a000 0680 0699  ...h.d4..A......
00000060: 0000 69a0 0000 1a00 1a0d 0034 0034 d3d4  ..i........4.4..
00000070: d1a3 d464 6834 6403 d469 b422 0d00 3400  ...dh4d..i."..4.
00000080: 1a68 068d 3403 4d06 8d00 0c80 00f5 0003  .h..4.M.........
00000090: 4031 3119 00d0 1a68 1a34 c86d 4640 00d0  @11....h.4.mF@..
000000a0: 0007 a80d 000d 00e9 a340 d034 0341 a000  .........@.4.A..
000000b0: 0699 07a9 881e a0d0 da80 6834 0c43 4068  ..........h4.C@h
000000c0: 6432 0340 0c80 6800 0346 8006 8000 d034  d2.@..h..F.....4
000000d0: 0001 f0e1 810e 1958 b7a4 92c7 640e 421a  .......X....d.B.
000000e0: a147 6142 a67e 3603 a756 3ba9 1b08 e034  .GaB.~6..V;....4
000000f0: 41fd 1247 661d b380 00b7 cd8c b23e b6b2  A..Gf........>..
00000100: 1947 e803 0be5 6077 a542 e9ea 7810 29f0  .G....`w.B..x.).
00000110: 429d e1d7 ad8b 0b78 056b e37c 06df 4917  B......x.k.|..I.
00000120: 9b46 f69d 4473 80b4 edc2 ee10 04e3 3e52  .F..Ds........>R
00000130: dd34 2244 08cb 5e64 9314 9521 505e e767  .4"D..^d...!P^.g
00000140: 9021 d029 85e7 9ce2 d1ce d44f 5ec5 f6d6  .!.).......O^...
00000150: d918 de31 f1f5 d149 4695 0937 d06b f046  ...1...IF..7.k.F
00000160: 789d 1bd0 ca69 11eb 2c9a 3290 3d9e 0511  x....i..,.2.=...
00000170: 6cad 205b edc8 c4b5 4691 379a 5978 58c3  l. [....F.7.YxX.
00000180: 4846 a4a0 3ba5 a89a a794 1f93 c588 8160  HF..;..........`
00000190: 016e 2504 2c74 643b 5046 4154 751c 33b1  .n%.,td;PFATu.3.
000001a0: c3e5 53d8 a959 5fdc 6c12 f2bd 02f3 2d83  ..S..Y_.l.....-.
000001b0: b965 3188 0d3c b097 4156 e950 9d49 64f6  .e1..<..AV.P.Id.
000001c0: da4a 2db5 a4ea 5365 27c0 1e79 8109 5f31  .J-...Se'..y.._1
000001d0: c184 46c9 74a5 f923 5ea1 6861 f058 226c  ..F.t..#^.ha.X"l
000001e0: 3df6 5d10 d11f d966 77c9 e488 448c 5a6f  =.]....fw...D.Zo
000001f0: 2c10 410b 4280 140a 0818 8afa 0cfa 8bf7  ,.A.B...........
00000200: ad34 3308 4077 6552 9849 378e 7d85 1fd8  .43.@weR.I7.}...
00000210: f287 1238 7639 11e2 f1e6 483b 7548 25e2  ...8v9....H;uH%.
00000220: 7de4 24ff 1a69 0b85 4b4c ebd0 1231 a512  }.$..i..KL...1..
00000230: f9fb 109c e7ea d932 98fd eb76 f4f8 fa29  .......2...v...)
00000240: 967c e152 9c69 c607 6207 eaef 2095 9441  .|.R.i..b... ..A
00000250: a64e 9ffc 5dc9 14e1 4241 ed3e 597c 9f2e  .N..]...BA.>Y|..
00000260: f0c8 4502 0000                           ..E...
```

- *xxd -r* es para realizar un reverse de un hexagecima 
- *sponge* es para que sobreescriba el archivo data con la nueva data

```bash
❯ cat data | xxd -r | sponge data
❯ cat data
       │ File: data   <BINARY>
```

- Los primeros bites al abrir ghex (magic numbers), list of signatures -> Cada tipo de archivo tiene su magic number

```bash
❯ file data
data: gzip compressed data, was "data2.bin", last modified: Sun Apr 23 18:04:23 2023, max compression, from Unix, original size modulo 2^32 581
❯ strings data
'sEd
data2.bin
BZh91AY&SY{O
dh4d
C@hd2
,td;PFATu
X"l=
@weR
H;uH%
❯ apt install ghex
❯ ghex data
```

- Le cambio el nombre al archivo por un .gz para poder descomprimirlo
```bash
❯ mv data data.gz
❯ 7z x data.gz
❯ 7z l data2.bin
```

- Con *7z l* podemos ver el tipo de archivo y que es lo que tiene en su interior y con x podemos extrar en su mayoria muchos tipos de compresiones

```bash
❯ 7z l data2.bin

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i5-7200U CPU @ 2.50GHz (806E9),ASM,AES-NI)

Scanning the drive for archives:
1 file, 581 bytes (1 KiB)

Listing archive: data2.bin

--
Path = data2.bin
Type = bzip2

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
                    .....                            data2
------------------- ----- ------------ ------------  ------------------------
                                                581  1 files
❯ 7z x data2.bin
```

- Realizamos el mismo proceso unas cuantas veces hasta encontrar el archivo ASCII que podremos leer con la conrtaseña
```bash
❯ ls # Los signos de interrogacion es porque no cargaron los logos que tengo en linuz para representar las carpetas
 scriptBash   data.gz   data2   data2.bin   data4.bin   data5.bin   data6   data6.bin   data8.bin   data9.bin   prueba   script.sh
❯ file data9.bin
data9.bin: ASCII text
```

```bash
❯ catn $! # !$ referencia al ultimo argumento escrito
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```

- Ahora veremos como se realiza mediante un script en bash para ganar un poco de agilidad

- Archivo .sh
```bash

#! /bin/bash

function ctrl_c(){
   echo -e "\n Saliendo ... \n"
   exit 1 # codigo de error
}

trap ctrl_c INT # capturar error

script_name="$0"
first_file_name="data.gz"
decompressed_file_name="$(7z l "$first_file_name" | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"

7z x "$first_file_name" &>/dev/null

while [ "$decompressed_file_name" ]; do
  echo -e "\n[+] Nuevo archivo descomprimido: $decompressed_file_name"
  if 7z x "$decompressed_file_name" &>/dev/null; then
    decompressed_file_name="$(7z l "$decompressed_file_name" 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
  else
    echo "Error en la descompresión: $decompressed_file_name"
    break
  fi
done

if [[ -f "$decompressed_file_name" ]]; then
  echo "Contenido del archivo:"
  cat "$decompressed_file_name"

  echo "Eliminando archivos..."
  for file in $(ls | grep -vE "$first_file_name|.sh$|9.bin"); do
    if [[ -f "$file" ]]; then
      rm "$file"
      echo "Archivo eliminado: $file"
    fi
  done
fi

echo "Archivos restantes:"
ls


```

```
❯ catn data9.bin
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```

![[Pasted image 20230712190021.png]]

---

#### Nivel 14

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

**Commands you may need to solve this level
ssh, telnet, nc, openssl, s_client, nmap**


- Primero verificamos si tenemos instalado ssh-server
```bash
❯ sudo apt install openssh-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
openssh-server is already the newest version (1:8.4p1-5+deb11u1).
openssh-server set to manually installed.
The following packages were automatically installed and are no longer required:
  gnome-desktop3-data graphicsmagick kitty-doc kitty-terminfo libgnome-desktop-3-19 libgraphicsmagick-q16-3 libmsgpackc2 libpipewire-0.3-0 libpipewire-0.3-common
  libpipewire-0.3-modules libspa-0.2-modules libtermkey1 libunibilium4 libvterm0 libwireplumber-0.4-0 libxkbregistry0 lua-luv neovim-runtime pipewire pipewire-bin pipewire-pulse
  python3-neovim python3-pynvim wireplumber
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

- Ahora realizaremos un ejemplo de conexion con clave publica y privada mediante ssh en mi localhost
- Verificamos el status en systemctl y si esta inactivo lo empezamos
```bash
❯ sudo systemctl status ssh
○ ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; disabled; preset: enabled)
     Active: inactive (dead)
       Docs: man:sshd(8)
             man:sshd_config(5)
❯ sudo systemctl start ssh
```

- En la siguiente ruta contaaremos con las claves privadas y publicas que se generen
```bash
❯ pwd
/home/ressc/.ssh
```

- Podemos ingresar ssh-keygen para generar en nuestro dispositivo
```bash
❯ sudo ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ressc/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ressc/.ssh/id_rsa
Your public key has been saved in /home/ressc/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:i6okdRXIgcE+6IKn+IQMHOwkTsSabL/2cFYM0hI1DXs ressc@parrot
The key's randomart image is:
+---[RSA 3072]----+
|.o.=o*+          |
|o.o = oo         |
|+O o +.E         |
|@o+ o.+          |
|=+o..  oS        |
|=+.o  .. .       |
|=+o..o. .        |
|o+ o+.           |
| .+.o.           |
+----[SHA256]-----+

```

- Luego con ls en la ruta numbrada anteriormente podemos ver con cat la *privada y la publica*
```bash
❯ ls
 id_rsa   id_rsa.pub   known_hosts
```

- Si creamos el archivo authorized_keys y le copiamos la clave publica podremos ingresar directamente de nuestro dispositivo sin ingresar la clave correspondiente
```bash
❯ cp id_rsa.pub authorized_keys
❯ ls
 authorized_keys   id_rsa   id_rsa.pub   known_hosts
❯ ssh ressc@127.0.0.1
Linux parrot 6.1.0-1parrot1-amd64 #1 SMP PREEMPT_DYNAMIC Parrot 6.1.15-1parrot1 (2023-04-25) x8664
 __                           _
|   \     _   | |  / _|     
| |) / ` | '| '/  | | _ \ /  / |
|  / (| | |  | | | () | |   __) |  / ( 
||   _,||  ||  ___/ _| |__/ _|___|




The programs included with the Parrot GNU/Linux are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Parrot GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jul 13 18:36:51 2023 from 127.0.0.1

❯ exit
Connection to 127.0.0.1 closed.

```

- Para conectarno con clave privada se realiza de la siguiente forma, este caso es porque en el nivel nos dan la clave privada en el mismo directorio
```bash
   ssh -i id_rsa [user ]@localhost # Para ingresar con clave privada

   bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
```

- Ahora al ingresar con el usuario bandit14 podemos ver la clave del nivel actual y podremos ingresar de la manera clasica
```bash
   bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
   fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```


Los pares de claves SSH son dos claves criptográficamente seguras que pueden usarse para autenticar a un cliente a un servidor SSH. Cada par de claves está compuesto por una clave pública y una clave privada.

El cliente mantiene la clave privada y debe mantenerla en absoluto secreto. Poner en riesgo la clave privada puede permitir a un atacante no autorizado iniciar sesión en los servidores que están configurados con la clave pública asociada sin autenticación adicional. Como medida de precaución adicional, es recomendable cifrar la clave en el disco con una frase de contraseña.

La clave pública asociada puede compartirse libremente sin ninguna consecuencia negativa. La clave pública puede usarse para cifrar mensajes que sólo la clave privada puede descifrar. Esta propiedad se emplea como forma de autenticación mediante el uso del par de claves.

---
#### NIvel 15

The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

**Commands you may need to solve this level
ssh, telnet, nc, openssl, s_client, nmap**

*Netcat* es una herramienta de línea de comandos que sirve para escribir y leer datos en la red. Para la transmisión de datos, Netcat usa los protocolos de red TCP/IP y UDP. La herramienta proviene originalmente del mundo de Unix; desde entonces, se ha expandido a todas las plataformas.

Gracias a su universalidad, a Netcat se la llama “la navaja suiza del TCP/IP”. Puede utilizarse, por ejemplo, para diagnosticar errores y problemas que afecten a la funcionalidad y la seguridad de una red. Netcat también puede escanear puertos, hacer streaming de datos o simplemente transferirlos. Además, permite configurar servidores de chat y de web e iniciar consultas por correo. Este software minimalista, desarrollado a mediados de los 90, puede operar en modo servidor y cliente.


- Podemos percatarnos que en el puerto 30000 existe algo corriedo y al ingresar la contraseña del nivel actual nos da la contraseña del nivel siguiente

```bash
bandit14@bandit:~$ nc localhost 30000
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```

![[Pasted image 20230713194318.png]]

![[Pasted image 20230713194417.png]]

![[Pasted image 20230713194636.png]]

- ps -faux # Lista todos los procesos del sistema
- ps -eo command # Listar los comandos especificos que se estan ejecutando en el sistema
---

#### Nivel 16

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

*Commands you may need to solve this level
ssh, telnet, nc, openssl, s_client, nmap*

- En primer lugar verifiamos mediante `which` que sirve para listar la ruta absoluta de un binario y asi podemos verificar si el comando existe en pocas palabras, si no te reporta nada el caso contrario que te reporte una ruta significa que no existe el binario
- Al verificar si esta instalado ncat podemos conectarnos de forma remota mediante ssl al puerto 30001 e ingresamos la contraseña 
- El comando `ncat --ssl`establece una conexión encriptada utilizando *SSL/TLS (Secure Sockets Layer/Transport Layer Security)*. Ncat es una herramienta de línea de comandos que proporciona capacidades de red, y al agregar la opción "--ssl", permite la comunicación segura entre el cliente y el servidor utilizando criptografía. Esta encriptación ayuda a proteger los datos transmitidos y asegura la confidencialidad e integridad de la información.

```bash
/usr/bin/ncat
bandit15@bandit:~$ ncat 
Ncat: You must specify a host to connect to. QUITTING.
bandit15@bandit:~$ ncat --ssl 127.0.0.1 30001
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1
```
---
#### Nivel 17

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

*Commands you may need to solve this level
ssh, telnet, nc, openssl, s_client, nmap*

- Para ver si los puertos estan abierto hay una ruta en el sistema, en una bash que es mas comodo que una zsh nos permite hacer esto

![[Pasted image 20230714191048.png]]

- Donde podemos apreciar mediante $? que el codigo de estado es 0 que indica que fue exitoso

![[Pasted image 20230714191155.png]]

- Con esto podemor ir jugando con los codigos de estado e ir concatenando and y or 

![[Pasted image 20230714191534.png]]

- Podemos enviar los errores al /dev/null y si vemos que no nos resulta envolvemos todo el comando pricipal en parentecis

![[Pasted image 20230714192003.png]]

- Con esto tenemos una via potencial para detectar si un puerto esta abierto o cerrado

- En cuanto a los hilo por otro lado podemos aceleras ciertas ejecuciones para que algunas se ejecuten en sudo plano
- En este caso pasaron unos segundo hasta que aparecio el mensaje

![[Pasted image 20230714193936.png]]

- Podemos trabajar con timeout para que su tiempo de vida sea limitado

![[Pasted image 20230714194038.png]]

- En primer lugar nos cremos un directorio temporal en /tmp para poder trabajar

```bash
bandit16@bandit:~$ ls
bandit16@bandit:~$ cd /temp/
-bash: cd: /temp/: No such file or directory
bandit16@bandit:~$ cd /tmp/ 
bandit16@bandit:/tmp$ ls
ls: cannot open directory '.': Permission denied

bandit16@bandit:/tmp$ mktemp -d
/tmp/tmp.N8qDslGxHY

bandit16@bandit:/tmp$ cd /tmp/tmp.N8qDslGxHY

bandit16@bandit:/tmp/tmp.N8qDslGxHY$ ls
```


- Nos entrega un listado de puertos en los cuales existe coneccion
- Al crearnos un archivo portScan.sh podemos interactuar dandole permisos de x
```bash
bandit16@bandit:/tmp/tmp.MMa3IqtBvR$ cat portScan.sh 
#!/bin/bash

ctrl_c(){
    echo "Saliendo..."
    exit 1;
}

trap ctrl_c INT

for port in $(seq 31000 32000);do
    (echo '' > /dev/tcp/127.0.0.1/$port) 2>/dev/null && echo "[+] $port - OPEN"
done
```

- Y al ejecutarlo podemos apreciar x catidad de puertos abiertos
```bash
bandit16@bandit:/tmp/tmp.MMa3IqtBvR$ ./portScan.sh 
[+] 31046 - OPEN
[+] 31518 - OPEN
[+] 31691 - OPEN
[+] 31790 - OPEN
[+] 31960 - OPEN
```

- Se puede realizar con nmap pero nos creamos un mini script en bash para aprender de mejor manera bash scriting
- Ahora con ncat vamos probando con cada uno proporciando la misma contraseña de bandit16 hasta que nos entregue la contraseña siguiente
```bash
bandit16@bandit:/tmp/tmp.MMa3IqtBvR$ ncat --ssl 127.0.0.1 31790
JQttfApK4SeyHwDlI9SXGR50qclOAil1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

- Podemos verificar que nos entrega una clave privada
- Nos creamos lo siguiente para utilizar la clave privada al momento de inicial al siguiente usuario y copiamos la clave privada en su interior
- Las claves privadas tienen que tener el permiso 600
```bash
bandit16@bandit:/tmp/tmp.MMa3IqtBvR$ nano id_rsa
chmod 600 id_rsa

````

- Nos conectamos mediante ssh pero tener en cuenta que siempre tenemos que pasar el puerto que se conecto anteriormente sino dara error
```bash
bandit16@bandit:/tmp/tmp.MMa3IqtBvR$ ssh -i id_rsa bandit17@127.0.0.1 -p 2220
```

- Una vez ingresado en esta ruta se almacenan todas las credenciales de cada user

![[Pasted image 20230714220049.png]]

- Y la podriamos ver su contenido
```bash
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e
```

---
#### Nivel 18

- Antes que nada mostrar dos scripts que creamos cada uno lista los puertos abiertos y ip abiertas

![[Pasted image 20230714220628.png]]

![[Pasted image 20230714220635.png]]

There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

**Commands you may need to solve this level
cat, grep, ls, diff**

- Si listamos el archivo e intentamos de contar las lineas nos percatamos que los dos tienen la misma cantidad *curioso*
```bash
bandit17@bandit:~$ ls
passwords.new  passwords.old

bandit17@bandit:~$ wc -l *
 100 passwords.new
 100 passwords.old
 200 total
```

- Si nos estan diciendo que existe una linea cambiada de old a new podemos utilizar *diff*
```bash
bandit17@bandit:~$ diff passwords.old passwords.new 
42c42
< glZreTEH1V3cGKL6g4conYqZqaEj0mte
---
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```

- Si nos intentamos conectar con la contraseña nos conecta y nos desconecta *Tiene truco*
- Pero eso es para el proximo nivel lo mas probable que tengamos que editar .bashrc para que no nos desconecte de golpe

 Comparar las diferencias con el comando diff: https://eltallerdelbit.com/comando-diff-ejemplos/

 ---

#### Nivel 19

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

**Commands you may need to solve this level
ssh, ls, cat**

A través del archivo de configuración ‘.bashrc‘ o ‘.zshrc‘, es posible definir una serie de acciones a llevar a cabo a la hora de obtener una consola interactiva, en este caso tras ingresar por SSH.

Es por ello que tras ingresar, somos expulsados de forma inmediata, dado que así ha sido definido en el archivo de configuración ‘.bashrc‘ para el caso que estamos tratando. Si colamos un comando al final de nuestra línea al aplicar una autenticación por SSH, lograremos que ese comando sea introducido a nivel de sistema antes de que se interprete el archivo de configuración pertinente.

- Podemos implementar un comando antes de que se interprete la .bashrc de este modo

![[Pasted image 20230714221925.png]]

- Y de ese mismo modo mandamos a llamar una bash 

```bash
❯ sshpass -p 'hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg' ssh bandit18@bandit.labs.overthewire.org -p 2220 bash
                                                
                        | |         | () | 
                        | ' \ /  | '_ \ / _ | | |
                        | |) | (| | | | | (| | | | 
                        |_./ _,|| ||_,||_|


                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

ls
readme
cat readme
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```

- Y obtenemos segun el enunciado que la contraseña del siguiente nivel estaba en el archivo readme

#### Nivel 20

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

##### Helpful Reading Material
[setuid on Wikipedia](https://en.wikipedia.org/wiki/Setuid)

- Al estar con un SUID, podemos realizar lo siguiente
```bash
bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
bandit19@bandit:~$ ./bandit20-do bash -p 
bash-5.1$ ls
bandit20-do
bash-5.1$ whoami
bandit20
```

```bash
bash-5.1$ ls
bandit20-do
bash-5.1$ ls -la
total 36
drwxr-xr-x  2 root     root      4096 Apr 23 18:04 .
drwxr-xr-x 70 root     root      4096 Apr 23 18:05 ..
-rwsr-x---  1 bandit20 bandit19 14876 Apr 23 18:04 bandit20-do
-rw-r--r--  1 root     root       220 Jan  6  2022 .bash_logout
-rw-r--r--  1 root     root      3771 Jan  6  2022 .bashrc
-rw-r--r--  1 root     root       807 Jan  6  2022 .profile
bash-5.1$ file bandit20-do 
bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=c148b21f7eb7e816998f07490c8007567e51953f, for GNU/Linux 3.2.0, not stripped
```

- Este archivo solo nos permitia conectarnos a bandit20 con Suid pero nos podemos ir la home de bandit20
```bash
bash-5.1$ pwd
/home/bandit19
bash-5.1$ cd /home/bandit20
bash-5.1$ pwd
/home/bandit20
bash-5.1$ ls
suconnect
````

- Pero la contraseña segun el enunciado esta en /etc/bandit_pass
```bash
bash-5.1$ cd /etc/bandit_pass
bash-5.1$ ls
bandit0   bandit12  bandit16  bandit2	bandit23  bandit27  bandit30  bandit4  bandit8
bandit1   bandit13  bandit17  bandit20	bandit24  bandit28  bandit31  bandit5  bandit9
bandit10  bandit14  bandit18  bandit21	bandit25  bandit29  bandit32  bandit6
bandit11  bandit15  bandit19  bandit22	bandit26  bandit3   bandit33  bandit7
bash-5.1$ cat bandit20
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```
- Y asi nos podemos conectar de la forma tradicional

SUID es un permiso de archivo especial para archivos ejecutables que permite a otros usuarios ejecutar el archivo con los permisos efectivos del propietario del archivo. SGID, por el contrario, es un permiso de archivo especial que también se aplica a los archivos ejecutables y permite a otros usuarios heredar el GID efectivo del propietario del grupo de archivos.

SUID significa “establecer ID de usuario” (Set owner User ID) y SGID significa “establecer ID de grupo” (Set Group ID up on execution).

#### Nivel 21

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE: Try connecting to your own network daemon to see if it works as you think**

***Commands you may need to solve this level
ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)***

- Si intentamos ejecutar el archivo podemos apreciar que necesita un puerto
```bash
bandit20@bandit:~$ ./suconnect 
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

- Al analizar el enunciado nos deberiamos de conectar dos veces con el mismo user y en una ventana nos ponemos en escucha en cualquier puerto que este disponible y nos conectamos
- Esto no deberia de dejar en espera para que coloquemos la contraseña

![[Pasted image 20230718143312.png]]

- Ingresamos por donde se dejo el puerto en escucha la contraseña del mismo nivel y nos manda la contraseña del siguiente nivel

```bash
NvEJF7oVjkddltPSrdKEFOllh9V1IBcq
```

Usando Netcat – Algunos comandos prácticos: https://blog.desdelinux.net/usando-netcat-algunos-comandos-practicos/

#### Nivel 21 -> 22

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

**Commands you may need to solve this level
cron, crontab, crontab(5) (use “man 5 crontab” to access this)**

Cron es un administrador de tareas de Linux que permite ejecutar comandos en un momento determinado, por ejemplo, cada minuto, día, semana o mes. Si queremos trabajar con cron, podemos hacerlo a través del comando crontab.

El formato de configuración de cron es el siguiente: Minuto Hora Dia-del-Mes Mes Dia-de-la-Semana Comando-a-Ejecutar

El intervalo de tiempo se especifica mediante 5 campos que representan, de izquierda a derecha:

Minutos: de 0 a 59.
Horas: de 0 a 23.
Día del mes: de 1 a 31.
Mes: de 1 a 12.
Día de la semana: de 1 a 6 lunes a sábado (1=lunes, 2=martes, etc.) y 0 o 7 el domingo.
Si quisiéramos especificar todos los valores posibles de un parámetro (minutos, horas, etc.) podemos hacer uso del asterisco (*). Esto implica que si en lugar de un número utilizamos un asterisco, el comando indicado se ejecutará cada minuto, hora, día de mes, mes o día de la semana, como en el siguiente ejemplo:

* * * * * /home/user/script.sh

```bash
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ 
```

- Entonces cada minuto el usuario esta asignando el privilegio 644 a la ruta mostrada anteriormente y hace un cat de lo que hay
- Si el usuario 22 puede ver su contraseña quizas nosotros podemos listar a donde la esta mandando `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`

```bash
bandit21@bandit:/etc/cron.d$ ls -la /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
-rw-r--r-- 1 bandit22 bandit22 33 Jul 18 18:47 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
```

- Y como cualquier usuario lo puede leer podremos leerlo nosotros
- Otra forma de detectarlo que se vera mucho mas adelante como pspy que permite motorizar los comandos que se esten ejecutando en el sistema a intervalos regulares de tiempo, pero tambien se pueden detectar creandonos un script propmont y seria otro forma de detectarlo

![[Pasted image 20230718145326.png]]

#### Nivel 22 -> 23

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

**Commands you may need to solve this level
cron, crontab, crontab(5) (use “man 5 crontab” to access this)**

```bash
bandit22@bandit:~$ cd /etc/cron.d/
bandit22@bandit:/etc/cron.d$ ls
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24       e2scrub_all  sysstat
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root  otw-tmp-dir
bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

- Realizamos un cat al archivo .sh para ver lo que hace
```bash
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

- md5sum es un hash 
- En el carpeta /tmp/ yo me tengo que saber los nombre ya que no los puedo listar

```bash
bandit22@bandit:~$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
```

Cron & Crontab, explicados: https://blog.desdelinux.net/cron-crontab-explicados/

#### Nivel 23 -> 24

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

**NOTE**: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2**: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

**Commands you may need to solve this level
cron, crontab, crontab(5) (use “man 5 crontab” to access this)**


AVISO: OverTheWire ha estado realizando una serie de cambios, comunicaros que ahora en vez de la ruta ‘/var/spool/bandit24/‘ tienes que emplear la ruta ‘/var/spool/bandit24/foo‘, por lo demás, todo sigue igual.

Esta parte es fundamental, sobre todo de cara a los módulos de Hacking que vamos a estar tocando en la academia, pues en muchas de las ocasiones veremos cómo nos será necesario no sólo identificar qué tareas se ejecutan en el sistema a intervalos regulares de tiempo, sino también saber cómo poder abusar de estas para elevar nuestros privilegios.

- Primero leemos el contenido del .sh para ver que deberiamos hacer para resolver el ejercicio
```bash
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo || exit 1
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -rf ./$i
    fi
done
```

- Creamos un directorio temporal para poder trabajar con mktemp -d 
- Tambien podemos crear una variable que me diga la direccion de mi carpeta temporal con dir_name=$(mktemp -d)

```bash
bandit23@bandit:/var/spool/bandit24/foo$ echo $dir_name 
/tmp/tmp.xcxPmN4cmS
```

- Creamos un script.sh para poder trabajar y damos permisos al archivo y la carpeta para que otros puedan escribir y leer
```bash
bandit23@bandit:/tmp/tmp.xcxPmN4cmS$ chmod +x script.sh
bandit23@bandit:/tmp/tmp.xcxPmN4cmS$ chmod o+wx /tmp/tmp.xcxPmN4cmS/
```

- En el archivo .sh escribimos lo siguiente
```bash
bandit23@bandit:/tmp/tmp.xcxPmN4cmS$ cat script.sh 
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.xcxPmN4cmS/bandit24_password.log
chmod o+r /tmp/tmp.xcxPmN4cmS/bandit24_password.log
```

- Luego copiamos el archivo al directorio que nos da el enunciado y nos podemos crear una carpeta para copiarlo en su interior
```bash
bandit23@bandit:/tmp/tmp.xcxPmN4cmS$ cp script.sh /var/spool/bandit24/foo/example
```

- Con watch -n 1 en cada segundo va monitorizando lo que va pasando en este directorio
```bash
bandit23@bandit:/tmp/tmp.xcxPmN4cmS$ watch -n 1 ls -l
```

- Y podemos apreciar que en unos segundo se escribe algo en el archivo que dirigimos el output, nos salimos y le realizamos un cat
```bash
bandit23@bandit:/tmp/tmp.xcxPmN4cmS$ cat bandit241_password.log
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar
```

- Y obtenemos la contraseña del siguiente nivel

---
### TAREAS CRON
- Para practicar las tareas Cron podemos utilzar la siguiente pagina 
   https://www.site24x7.com/es/tools/crontab/cron-generator.html

![[Pasted image 20230721105840.png]]

- Minuto

![[Pasted image 20230721111104.png]]

- Hora
   */  : Every - Cada

![[Pasted image 20230721111157.png]]
![[Pasted image 20230721111254.png]]
![[Pasted image 20230721111608.png]]

- Dia del mes

![[Pasted image 20230721112207.png]]
![[Pasted image 20230721112258.png]]

- Mes

![[Pasted image 20230721112326.png]]
![[Pasted image 20230721112347.png]]

- Dia de la semana , Domingo es un 0

![[Pasted image 20230721112436.png]]
![[Pasted image 20230721112539.png]]

![[Pasted image 20230721113141.png]]


La L significa el ultimo valor comprendido en el apartado que se este realizando (Solo es valido para el dia del mes y para el dia de la semana)

![[Pasted image 20230721112745.png]]

Tambien se puede escribir el mes y dia de la forma convencional

![[Pasted image 20230721112916.png]]

---

#### Nivel 24 -> 25

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

- Primero nos creamos un directorio temporal de trabajo

```bash
dir_name=$(mktemp -d)
cd $dir_name # Nos movemos al directorio para poder tener los permisos
```

- Posterormente entendemos que debemos de realizar

```
nc localhost 30002
```

- Aqui me solicita la contaseña actual y separada con un espacio los 4 digitos que deberiamos de ralizar fuerza bruta
- Primero realizamos un for para comprender el funcionamiento
```bash
for i in {0000..9999}; do echo $i; done
```

- Una vez entendido de que es lo que se realizara podemos crear un archivo con todas la combinaciones posibles donde i sera el numero de 4 digitos y lo mandamos a un archivo .txt

```bash
bandit24@bandit:/tmp/tmp.HUkuXmAdl0$ for i in {0000..9999};do echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i"; done > combination.txt
```

- Despues realizamos la fuerza bruta con **cat** y **grep** para eliminar las lineas no deseadas
```bash
bandit24@bandit:/tmp/tmp.HUkuXmAdl0$ cat combination.txt | nc localhost 30002 | grep -vE "Wrong|Please"
```

- Se puede apreciar que con **cat** es posible que nos quedemos sin recursos en el servidor pero podriamos utilzar **tac** para realizar el cat inversmante eso significa de abajo a arriba
```bash
bandit24@bandit:/tmp/tmp.HUkuXmAdl0$ tac combination.txt | nc localhost 30002 | grep -vE "Wrong|Please"
Correct!
The password of user bandit25 is p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d

Exiting.
```


Otra forma válida de poder haber resuelto este ejercicio habría sido crear un script que por cada iteración que se efectúe en el secuenciador, se pruebe una conexión enviando los datos que le pasamos nuestro lado, correspondiente a la cadena y al PIN.

Lo único a tener en cuenta es que en caso de no usar hilos, es posible que se experimente cierta lentitud a la hora de correr el script siguiendo este principio.

- Si ingresamos al siguiente nivel tenemos el .pin donde nos muestra el pin de inicio anterior

**9708**

---

#### Nivel 25 -> 26 -> 27

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

**Commands you may need to solve this level
ssh, cat, more, vi, ls, id, pwd**

- Con el comando more existe un modo visual en el cual podemor realizar cosas
- Si visualizamos las credenciales de bandit26 podemos apreciar que no es una bash y al conectarnos no expulsara de nuevo

```bash
bandit25@bandit:~$ cat /etc/passwd | grep bandit26 
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

- Si achicamos la ventana nos aparecera la funcion del more y si ingresamos a modo visial podemos crear variables
```bash
:set shell=/bin/bash
```

- Luego ingresamos enter escape y despues ingresamos de la misma manera shell y estaremos en una terminal con el usuario bandit26
```bash
:shell
```

¡A veces el tamaño sí que importa!, aislado a esto que hemos visto, el comando ‘more‘ lo que te permite es mostrar el resultado de la ejecución de un comando en la terminal de a una página a la vez. Esto es especialmente útil para casos en los que se ejecutan comandos que puedan llegar a causar un gran desplazamiento.

Para pasar a la página siguiente, tienes que presionar la barra espaciadora en el teclado.

Puedes continuar presionando espacio hasta llegar al final del resultado o puedes presionar la tecla “q” directamente para salir.

---


```bash
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@127.0.0.1 -p 2220
```


- Antes de pasar al otro nivel nos combiene trabajar aqui mismo para obtener las credenciales de bandit28 ya que tenemos un ejecutable SUID y podemos ejecutar comandos como el propietario de forma temporal

```bash
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
```

---

#### Nivel 27 -> 28

There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level
git**

- En primer lugar nos creamos un directorio temporal e ingresamos en el `mktem -d`
- Dentro de la carpeta temporal clonamos el repositorio entregado, recordar agregar el puerto correspondiente
```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```

- Nos solicita contraseña pero en el enunciado nos indica que es la misma contraseña que ingresamos a bandit27
- Luego al clonar el repo podremos ver que en un README.md se encuentra la pass del siguiente nivel
```bash
bandit27@bandit:/tmp/tmp.7gjSST9WdT$ cd repo/
bandit27@bandit:/tmp/tmp.7gjSST9WdT/repo$ ls
README
bandit27@bandit:/tmp/tmp.7gjSST9WdT/repo$ cat README 
The password to the next level is: AVanL161y9rsbcJIsFHuw35rjaOM19nR
```

**Github** es un portal creado para alojar el código de las aplicaciones de cualquier desarrollador, y que fue comprada por Microsoft en junio del 2018. La plataforma está creada para que los desarrolladores suban el código de sus aplicaciones y herramientas, y que como usuario no sólo puedas descargarte la aplicación, sino también entrar a su perfil para leer sobre ella o colaborar con su desarrollo.

También tiene un sistema de seguimiento de problemas, para que otras personas puedan hacer mejoras, sugerencias y optimizaciones en los proyectos. Ofrece también una herramienta de revisión de código, de forma que no sólo se puede mirar el código fuente de una herramienta, sino que también se pueden dejar anotaciones para que su creador o tú mismo las podáis revisar. Se pueden crear discusiones también alrededor de estas anotaciones para mejorar y optimizar el código.


---

#### Nivel 28 -> 29

There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level
git**

Con el comando **git log** podremos siempre listar todos los Commits existentes en un proyecto. Lo que obtendremos es un listado de identificadores los cuales podremos aprovechar para, por ejemplo, mediante el uso del comando **git show** seguido del identificador del **Commit** cuyas propiedades queramos visualizar, ser capaces de ver todos los cambios que se hayan aplicado para un punto dado del proyecto.

En caso de querer volver a un estado del proyecto pasado, podremos hacer uso del comando **git checkout** seguido del identificador del **Commit** **deseado**, pudiendo así volver a ese punto de la historia con todos los archivos existentes para ese entonces restaurados.


- Nos creamos un directorio temporal para poder trabajar en el
- Clonamos el repositorio nombrado de la misma forma anterior y visializamos lo de su interior
```bash
bandit28@bandit:/tmp/tmp.UeOEH1jSUi$ git clone  ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
```

```bash
bandit28@bandit:/tmp/tmp.UeOEH1jSUi/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

- Visualizamos los log del repositorio
```bash
bandit28@bandit:/tmp/tmp.UeOEH1jSUi/repo$ git log
commit 899ba88df296331cc01f30d022c006775d467f28 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Sun Apr 23 18:04:39 2023 +0000

    fix info leak

commit abcff758fa6343a0d002a1c0add1ad8c71b88534
Author: Morla Porla <morla@overthewire.org>
Date:   Sun Apr 23 18:04:39 2023 +0000

    add missing data

commit c0a8c3cf093fba65f4ee0e1fe2a530b799508c78
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:39 2023 +0000

    initial commit of README.md
```

- Se puede apreciar que en el segundo commit agregan missing data entonces prodriamos realizar un checkout para ver los cambios
```bash
bandit28@bandit:/tmp/tmp.UeOEH1jSUi/repo$ git checkout abcff758fa6343a0d002a1c0add1ad8c71b88534

bandit28@bandit:/tmp/tmp.UeOEH1jSUi/repo$ cat README.md 
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S

```

- Y se puede apreciar que obtenemos las credenciales del siguiente nivel
- Tambien con `git show [commit]` podemos ver los cambiar realizados en el commit

---

#### Nivel 29 -> 30

There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level
git**


El comando **git branch** nos permite crear, enumerar y eliminar ramas, así como cambiar sus nombres. Es importante recalcar que no nos permite cambiar entre ramas o volver a unir un historial bifurcado. Por este motivo, **git branch** está estrechamente integrado con los comandos **git checkout** y **git merge**.

- La misma historia, creamos un directorio temporal para poder trabajar
- Luego nos clonamos el repositorio
```bash
bandit29@bandit:~$ mktemp -d 
/tmp/tmp.YqjzYrKmhE
bandit29@bandit:~$ cd $!
bandit29@bandit:~$ cd /tmp/tmp.YqjzYrKmhE
bandit29@bandit:/tmp/tmp.YqjzYrKmhE$ ls
bandit29@bandit:/tmp/tmp.YqjzYrKmhE$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit29/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit29/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit29-git@localhost's password: 
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
```

- Cateamos el readme
```bash
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

- Intentamos visualizar los logs e visualizamos el pertinente
```bash
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git log
commit 4bd5389f9f2b9e96ba517aa751ee58d051905761 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    fix username

commit 1a57cf10158f133c4f40ff82251f605a7618631d
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    initial commit of README.md

bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git show 4bd5389f9f2b9e96ba517aa751ee58d051905761
commit 4bd5389f9f2b9e96ba517aa751ee58d051905761 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.
 
 ## credentials
 
-- username: bandit29
+- username: bandit30
 - password: <no passwords in production!>
```

- Podriamos visualizar las ramas existentes con `git branch -a`
```bash 
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

- Ahora podriamos ver el contenido de las ramas, cambiandonos con `git checkout [rama]`

```bash
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git checkout dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
Switched to a new branch 'dev'
```

- Podemos listar todas las ramas nuevamente y ver que estamos en la rama dev
```bash
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git branch -a
* dev
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

- Listamos los commits realizados con `git log`
```bash
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git log
commit 13e735685c73e5e396252074f2dca2e415fbcc98 (HEAD -> dev, origin/dev)
Author: Morla Porla <morla@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    add data needed for development

commit 8caf551dba9f9e39bc8ea4163de7902e6fa85f3a
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    add gif2ascii

commit 4bd5389f9f2b9e96ba517aa751ee58d051905761 (origin/master, origin/HEAD, master)
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    fix username

commit 1a57cf10158f133c4f40ff82251f605a7618631d
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    initial commit of README.md
```

- Podriamos listar cual estimes conveniente para ver si encontramos la pass del siguiente nivel

```bash
bandit29@bandit:/tmp/tmp.YqjzYrKmhE/repo$ git show 13e735685c73e5e396252074f2dca2e415fbcc98
commit 13e735685c73e5e396252074f2dca2e415fbcc98 (HEAD -> dev, origin/dev)
Author: Morla Porla <morla@overthewire.org>
Date:   Sun Apr 23 18:04:40 2023 +0000

    add data needed for development

diff --git a/README.md b/README.md
index 1af21d3..a4b1cf1 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for bandit30 of bandit.
 ## credentials
 
 - username: bandit30
-- password: <no passwords in production!>
+- password: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS

```

- Podemos ver que se realizo el cambio

---

#### Nivel 30 -> 31

There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level
git**

En Git, una **etiqueta** o **tag** sirve básicamente como una **rama firmada que no permuta, es decir, siempre se mantiene inalterable**. Sencillamente es una cadena arbitraria que apunta a un **Commit específico**. Puede decirse que un tag es un nombre que puedes usar para marcar un punto específico en la historia de un repositorio.

- Creamos un directorio temporal y realizamos lo mismo que los casos anteriores
```bash
bandit30@bandit:/tmp/tmp.x4KASLZLSt/repo$ cat README.md 
just an epmty file... muahaha
bandit30@bandit:/tmp/tmp.x4KASLZLSt/repo$ ls
README.md
bandit30@bandit:/tmp/tmp.x4KASLZLSt/repo$ git log
commit 59530d30d299ff2e3e9719c096ebf46a65cc1424 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Sun Apr 23 18:04:42 2023 +0000

    initial commit of README.md
bandit30@bandit:/tmp/tmp.x4KASLZLSt/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

- No vemos nada apreciable a simple vista pero podriamos listar las tags
```bash
bandit30@bandit:/tmp/tmp.x4KASLZLSt/repo$ git tag
secret
```

- Vemos una tag secreta
- Con `git show [nombre del tag]` podemos catear su contenido
```bash
bandit30@bandit:/tmp/tmp.x4KASLZLSt/repo$ git show secret 
OoffzGDlzhAlerFJ2cAiz1D41JW1Mhmt
```

---

#### Nivel 31 -> 32

There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

**Commands you may need to solve this level
git**

Los **commits** son la base principal del trabajo de Git, ya que es el comando más usado para guardar cualquier cambio en esta herramienta. Si te preguntas qué es un commit, te puedes hacer una idea al entenderlo como una captura de pantalla del trabajo que haces cada segundo en Git, creando en consecuencia una versión del proyecto en el repositorio local.

- Realizamos lo mismo que los procedimientos anteriores
- Catemos el contendio y ahora nos dice que nuestra tarea es realizar un push a file en un repositorio remoto
```bash
bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ cat README.md 
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

- Cramo el archivo key.txt con su contenido
```bash
bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ git tag
bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ nano key.txt 
Unable to create directory /home/bandit31/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ ls
key.txt  README.md
```

- Agregamos el archivo al repositorio local y ingreamos un commit como toda la vida
- Posteriormente realizamos el push, nos solicita la contraseña del nivel anterior e visiaulizamos el cambio
```bash
bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ git add -f key.txt 
bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ git commit -m "KEY"
[master 8345722] KEY
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
bandit31@bandit:/tmp/tmp.hBdmdMMZL6/repo$ git push -u origin master 
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit31/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password: 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 319 bytes | 319.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: rmCBvG56y58BXzv98yZGdO7ATVL5dW8y 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://localhost:2220/home/bandit31-git/repo

```

- Y se puede apreciar la contraseña para el siguiente nivel

---

#### Nivel 32 -> 33 Final

After all this git stuff its time for another escape. Good luck!

**Commands you may need to solve this level
sh, man**

- Podemos apreciar que si nos deja ejecutar variables de entorno  `$HOME`
```bash
>> $HOME
sh: 1: /home/bandit32: Permission denied
```

En Bash se pueden usar argumentos desde la línea de comandos, los cuales son enviados a los scripts como variables. Estos quedarían representados de la siguiente forma:

[$0]: Representa el nombre del script que se invocó desde la terminal.

[$1]: Es el primer argumento desde la línea de comandos.

[$2]: Es el segundo argumento desde la línea de comandos y así sucesivamente.

[$#]: Contiene el número de argumentos que son recibidos desde la línea de comandos.

[$*]: Contiene todos los argumentos que son recibidos desde la línea de comandos, guardados todos en la misma variable.

- Al ver el home podemos apreciar que nos lanza una sh como output 
- En este caso si ejecutamos `$0` nos abrirar la sh para poder trabajar

- Si visualizamos el tipo de archivo nos damos cuenta que es un binario compilado
```bash
$ ls    			
uppershell
$ file uppershell
uppershell: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=8864473908fa10566d408e55323058f479337c0b, for GNU/Linux 3.2.0, not stripped
```

- Primero visualizamos donde nos encontramos y nos podriamos dirigir al home de bandit33
```bash
$ pwd
/home/bandit32
$ cd /home/bandit32
$ cd /home/bandit33                     
$ ls
README.txt
$ cat README.txt
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```




