
**GitHub** ya no admite la autenticación con contraseña para las operaciones de Git a través del protocolo HTTPS. A partir del 13 de agosto de 2021, GitHub requiere que uses métodos de autenticación más seguros, como el token de acceso personal (Personal Access Token) o el uso de claves SSH.

Es necesario utilizar un token de acceso personal o configurar el acceso a través de claves SSH.

### Opción 1: Usar un token de acceso personal

1. Ve a tu cuenta de **GitHub** en el navegador y accede a `"Settings" (Configuración) > "Developer settings" (Configuración de desarrollador) > "Personal access tokens" (Tokens de acceso personal)`.
2. Haz clic en "**Generate new token**" (Generar nuevo token) y sigue las instrucciones para configurar el token. Asegúrate de seleccionar los permisos necesarios según lo que desees hacer con el repositorio.
3. Copia el token generado.

Ahora, en tu máquina local, recordar tener instalado **git**

4. En la terminal, ejecuta el siguiente comando para configurar tu nombre de usuario de GitHub globalmente:
```
git config --global user.name "TuNombreDeUsuario"
```
5. A continuación, ejecuta el siguiente comando para configurar el token de acceso personal como contraseña en Git:
```
git config --global user.password TuTokenDeAccesoPersonal
```

Reemplaza "TuNombreDeUsuario" con tu nombre de usuario de GitHub y "TuTokenDeAccesoPersonal" con el token que copiaste anteriormente.

Ahora deberías poder realizar el push sin problemas. 

- Existe la posibilidad de que al realizar push solicite nombre de usuario y password, debes de ingresar nuevamente el personal access token. Tener en cuenta que esta contraseña es temporar y de inicio unico, puedes generar mas cuendo estimes conveniente.

### Opción 2: Configurar el acceso con claves SSH

1. Verifica si ya tienes una clave SSH configurada en tu máquina local. En la terminal, ejecuta el siguiente comando:
```
ls ~/.ssh/
```
2. Si ves archivos como `id_rsa` y `id_rsa.pub`, eso significa que ya tienes una clave SSH configurada. En caso contrario, debes generar una nueva clave SSH.
3. Si necesitas generar una nueva clave SSH, ejecuta el siguiente comando en la terminal:
```
ssh-keygen -t rsa -b 4096 -C "tu_correo@ejemplo.com"
```
Sustituye "tu_correo@ejemplo.com" con tu dirección de correo electrónico asociada a tu cuenta de GitHub. Presiona "Enter" para confirmar la ubicación y establecer una contraseña (opcional) para la clave.
4. Una vez que tengas una clave SSH configurada, copia el contenido de la clave pública (`id_rsa.pub`).
```
cat ~/.ssh/id_rsa.pub
```
5. Ahora ve a tu cuenta de GitHub en el navegador y accede a "Settings" (Configuración) > "SSH and GPG keys" (Claves SSH y GPG) > "New SSH key" (Nueva clave SSH).
6. Pega el contenido de la clave pública en el campo correspondiente y dale un título descriptivo a la clave.
7. Guarda la clave.

Ahora, en tu máquina local:

8. Configura tu nombre de usuario de GitHub globalmente (si no lo has hecho ya):
```
git config --global user.name "TuNombreDeUsuario"
```
9. Configura el acceso a GitHub a través de SSH en lugar de HTTPS:
```
git remote set-url origin git@github.com:TuNombreDeUsuario/TuRepositorio.git
```
Reemplaza "TuNombreDeUsuario" con tu nombre de usuario de GitHub y "TuRepositorio" con el nombre de tu repositorio en GitHub.

Con esto, deberías poder realizar un push sin problemas utilizando SSH como protocolo de autenticación.

Espero que esta información te sea útil. Si tienes alguna otra pregunta o problema, no dudes en preguntar. ¡Buena suerte con tu proyecto en GitHub!