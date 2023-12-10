
Overview – Kitty: https://sw.kovidgoyal.net/kitty/overview/

Vim Cheat Sheet: https://vim.rtorr.com/

### Requisitos

- Contar con una maquina windows 11 de preferencia 
- VMware 17 Pro para poder realizar snapshots
- Key VMware Pro = 4A4RR-813DK-M81A9-4U35H-06KND
- Parrot OS Security Edition = ISO (amd64)
- Recordar que al iniciar VMware colocar pantalla completa porque si no lo haces se vera mal la resolucion

### Instalacion Sistema Operativo VMware Pro

1. Abrir y montar Parrot OS en VMware 
	- Despues de configurar la memoria y los procesadores en mi caso 4 y 4
	- En network adapter seleccionamos bridged y seleccionamos replicate physical network
	- Acelerate 3D graphics si la maquina funciona lento
2. Ejecutamos la maquina y continuamos con la instalacion del sistema operativo
	- Recordar cifrar el disco
	- Eliminar todos los datos de disco y proceder con la instalacion
3. Realizar una snapshot cuando se termine la instalacion por si es necesario volver a un paso anterior
	- Puedes eliminar la campana de la terminal en edit -> Profile Preferences -> Terminal Bell
	- Y el cursor shape -> I-Beam 
	- Verificar si tengo hostname -I y asegurar que tengas direccion IP, tambien puedes ver con ifconfig
	- Verificar su puedo mandar paquetes con DNS -> ping -c 1 google.com

### Configuracion de Entorno

#### Instalando Bspwm y Sxhkd

- Si queremos ir a una snapshot anterior solo realizamos el mismo procedimiento para realizar una snapshot y seleccionamos a cual queremos de regresar
- Primero realizar parrot-upgrade (NO RELIAZAR SUDO APT UPGRADE PARROT NO ESTA PREPARADO PARA ESO) todo esto con el usuario root (sudo su -> para acceder e ingresa tu clave de inicio de sesion)
- Primero instalar todos lo paquetes necesarios para instalar Bspwm y Sxhkd

```zsh
apt install build-essential git vim xcb libxcb-util0-dev libxcb-ewmh-dev libxcb-randr0-dev libxcb-icccm4-dev libxcb-keysyms1-dev libxcb-xinerama0-dev libasound2-dev libxcb-xtest0-dev libxcb-shape0-dev
```

- Posteriormente se realiza en el mismo usuario root un apt update y al realizar un exit salimos del usuario root y nos vamos a la carpeta ~/Downloads para clonar los siguientes repositorios

```zsh
git clone https://github.com/baskerville/bspwm.git
git clone https://github.com/baskerville/sxhkd.git
```

- Para instalar cada uno de estos, ingresamo en ambos directorios por separado y ejecutar los comandos ‘**make**‘ y ‘**sudo make install**‘.

#### Configurando el Sxhkd
- Vamos a crear dos archivos 
	- mkdir ~/.config/{bspwm, sxhkd}
	- Despues en descargas nos vamos al directorio bspwm/examples
	- Y miraremos bspwmrc y sxhkdrc que son los archivos de configuracion de cada uno
	- copiamos cada uno en su respectivo directorio de .config
		```zsh
		cp bspwmrc ~/.config/bspwm/
		cp sxhkdrc ~/.config/sxhkd/
		```

- Instalamos kitty pero no la version definitiva, entramos al root y realizamos apt install kitty -y -> la cual es un tipo de terminal full personalizable
- Despues vamos a editar ~/.config/sxhdk/sxhdkrc y editamos lo siguiente:

```
# terminal emulator
super + Return
	kitty

# program launcher
super + @space
	dmenu_run

# make sxhkd reload its configuration files:
super + Escape
	pkill -USR1 -x sxhkd

#
# bspwm hotkeys
#

# quit/restart bspwm
super + alt + {q,r}
	bspc {quit,wm -r}

# close and kill
super + {_,shift + }w
	bspc node -{c,k}

# alternate between the tiled and monocle layout
super + m
	bspc desktop -l next

# send the newest marked node to the newest preselected node
super + y
	bspc node newest.marked.local -n newest.!automatic.local

# swap the current node and the biggest window
super + g
	bspc node -s biggest.window

#
# state/flags
#

# set the window state
super + {t,shift + t,s,f}
	bspc node -t {tiled,pseudo_tiled,floating,fullscreen}

# set the node flags
super + ctrl + {m,x,y,z}
	bspc node -g {marked,locked,sticky,private}

#
# focus/swap
#

# focus the node in the given direction
super + {_,shift + }{Left,Down,Up,Right}
	bspc node -{f,s} {west,south,north,east}

# focus the node for the given path jump
super + {p,b,comma,period}
	bspc node -f @{parent,brother,first,second}

# focus the next/previous window in the current desktop
super + {_,shift + }c
	bspc node -f {next,prev}.local.!hidden.window

# focus the next/previous desktop in the current monitor
super + bracket{left,right}
	bspc desktop -f {prev,next}.local

# focus the last node/desktop
super + {grave,Tab}
	bspc {node,desktop} -f last

# focus the older or newer node in the focus history
super + {o,i}
	bspc wm -h off; \
	bspc node {older,newer} -f; \
	bspc wm -h on

# focus or send to the given desktop
super + {_,shift + }{1-9,0}
	bspc {desktop -f,node -d} '^{1-9,10}'

#
# preselect
#

# preselect the direction
super + ctrl + alt + {Left,Down,Up,Right}
	bspc node -p {west,south,north,east}

# preselect the ratio
super + ctrl + {1-9}
	bspc node -o 0.{1-9}

# cancel the preselection for the focused node
super + ctrl + alt +  space
	bspc node -p cancel

# cancel the preselection for the focused desktop
super + ctrl + shift + space
	bspc query -N -d | xargs -I id -n 1 bspc node id -p cancel

#
# move/resize
#

# expand a window by moving one of its side outward
#super + alt + {h,j,k,l}
#	bspc node -z {left -20 0,bottom 0 20,top 0 -20,right 20 0}

# contract a window by moving one of its side inward
#super + alt + shift + {h,j,k,l}
#	bspc node -z {right -20 0,top 0 20,bottom 0 -20,left 20 0}

# move a floating window
super + shift + {Left,Down,Up,Right}
	bspc node -v {-20 0,0 20,0 -20,20 0}

# Custom Resize

alt + super + {Left,Down,Up,Right}
	/home/ressc/.config/bspwm/scripts/bspwm_resize {west,south,north,east}

```

- Posteriormente creamos el archivo de la ultima linea del codigo anterior y con alt . podemos ver la utima ruta que se ejecuto
```zsh
mkdir /home/ressc/.config/bspwm/scripts/
touch /home/ressc/.config/bspwm/scripts/bspwm_resize
# Para darle permisos de ejecucion
chmod +x !$ 
```

- Posteriormente con nano ingresamos esto al archivo creado:
```zsh
#!/usr/bin/env dash

if bspc query -N -n focused.floating > /dev/null; then
	step=20
else
	step=100
fi

case "$1" in
	west) dir=right; falldir=left; x="-$step"; y=0;;
	east) dir=right; falldir=left; x="$step"; y=0;;
	north) dir=top; falldir=bottom; x=0; y="-$step";;
	south) dir=top; falldir=bottom; x=0; y="$step";;
esac

bspc node -z "$dir" "$x" "$y" || bspc node -z "$falldir" "$x" "$y"
```
#### Instalacion Polybar

- instalar una serie de dependencias previas a la instalación de la Polybar con usuario root

```zsh
apt install cmake cmake-data pkg-config python3-sphinx libcairo2-dev libxcb1-dev libxcb-util0-dev libxcb-randr0-dev libxcb-composite0-dev python3-xcbgen xcb-proto libxcb-image0-dev libxcb-ewmh-dev libxcb-icccm4-dev libxcb-xkb-dev libxcb-xrm-dev libxcb-cursor-dev libasound2-dev libpulse-dev libjsoncpp-dev libmpdclient-dev libuv1-dev libnl-genl-3-dev
```

- Despues nos vamos a la carpeta de descargas para clonar el siguiente repositorio en forma recursiva
```zsh
git clone --recursive https://github.com/polybar/polybar
```

- Ingresamos al directorio creado y colocamos los siguiente comandos
```zsh
mkdir build
cd build
cmake ..
make -j$(nproc)
sudo make install
```

- Lo bueno de realizarlo de esta forma que nos aseguramos que la Polybar este en su ultima version y visualizamos si esta como binario con -> which polybar 
- No deberia de haber problemas en este apartado


#### Instalación de Picom y retocando el sxhkdrc

- Ejecutar para instalar ciertas dependencias previas en usuario root, recordar que siempre que instalamos paquetes es utilizando sudo o directamente en root
- control + k para limpiar la terminar, es lo mismo que escribir clear
```zsh
apt install meson libxext-dev libxcb1-dev libxcb-damage0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-render-util0-dev libxcb-render0-dev libxcb-composite0-dev libxcb-image0-dev libxcb-present-dev libxcb-xinerama0-dev libpixman-1-dev libdbus-1-dev libconfig-dev libgl1-mesa-dev libpcre2-dev libevdev-dev uthash-dev libev-dev libx11-xcb-dev libxcb-glx0-dev
```

- Luego nos vamos a descargas y clonamos el siguiente repositorio
```zsh
git clone https://github.com/ibhagwan/picom.git
```

- Una vez realizado ingresamos al directorio creado y ingresamos estos comandos por la terminal

```zsh
git submodule update --init --recursive
meson --buildtype=release . build
ninja -C build
sudo ninja -C build install
```

- Despues ingresamos como root e instalamos rofi
```zsh
apt install rofi
```

- Y editamos ~/.config/sxhkd/sxhkdrc solo el prgram launcher
```zsh
# program launcher
super + d
	rofi -show run
```

- Y finalmente intalamos bspwm en root
```zsh
apt install bspwm
```

- Si ingresamos por terminal kill -9 -1 cerramos la sesion
- Y luego ingresamos al apartado de bspwm en el circulo blanco con nuestra contraseña
- Algunos comando basicos ya que ingresaras a una pantalla en negro dado que aun no configuramos nada
	- super + enter -> Abrimos una terminal (kitty)
	- super + d -> abrimos rofi 
	- super + s -> ventana flotante 
	- super + click izquierdo -> movemos la ventana
	- super + click derecho -> ajustar el tamaño
	- super + alt + {Arriba, Abajo, Izquierda, Derecha} -> Esto con el script que creamos
	- super + t -> Coloca la ventana como estaba
	- super + f -> modo pantalla completa pero no se utiliza ya veras porque
	- ctrl + super + alt +  {Arriba, Abajo, Izquierda, Derecha} -> preselector de ventana a utilizar
	- ctrl + super + alt + spacio -> cancela el preselector
	- ctrl + super + 1,2,3,4... -> Para ajustar el tamaño del preselector
	- super + enter -> y abrimos la kitty en el preselector 
	- super + {Arriba, Abajo, Izquierda, Derecha} -> Para movernos de ventana en el espacio de trabajo que se marca en blanco
	- super + w -> para cerrar la ventana en la cual estamos posicionados pero se cambiara por super + q 
	- super + esc -> para que se actualicen los cambios que se realizaron en bspwm
	- ctrl + shift + enter -> abre una nueva consola en la misma kitty
	- super + f -> abrir firefox esto se configura enseguida
	- super + shift + q -> salir a la pantalla de bloqueo
	- ctrl + shift + t -> abre un nuevo entorno de trabajo
	- ctrl + shift + alt + t -> para renombrar el entorno de trabajo
	
- agregamos estas configuraciones a .config/sxhkd/sxhkdrc

	```zsh
	# quit/restart bspwm
	super + shift + {q,r}
		bspc {quit,wm -r}
	# close and kill
	super + {_,shift + }q
		bspc node -{c,k}
	# open firefox al final del todo
	super + shift + f
		usr/bin/forefox # esta ruta se ve con which firefox
```

- Y apretamos super + shift + escape para que se actualicen los cambios

#### Configurando las fuentes, la Kitty e instalación de Feh

- Nos descargamos en ~/Download la fuente Hack Nerd Font de nerdfonts en cual sera un formato .zip
- Nos colocamos en usuario root y movemos el archivo con este comando a la siguiente ruta

```zsh
mv Hack.zip /usr/local/share/fonts/
cd /usr/local/share/fonts/
unzip Hack.zip
rm Hack.zip
```

- Ahora configuramos la clipboard multidireccional y nos salimos del usuario root
```zsh
nano ~/.config/bspwm/bspwmrc
```

- Y al final del todo ingresamos (& es para que este en segundo plano) y luego apretamos super + shift + r para que cargue la nueva configuracion
```zsh
vmware-user-suid-wrapper &
```

- Despues en el usuario root instalamos zsh -> apt install zsh
- Luego nos dirigimos a ~/.config/kitty/ y creamos con nano kitty.conf y nano color.ini
```zsh 
# kitty.conf no escribas esta linea
enable_audio_bell no

include color.ini

font_family HackNerdFont
font_size 13

disable_ligatures never

url_color #61afef

url_style curly

map ctrl+left neighboring_window left
map ctrl+right neighboring_window right
map ctrl+up neighboring_window up
map ctrl+down neighboring_window down

map f1 copy_to_buffer a
map f2 paste_from_buffer a
map f3 copy_to_buffer b
map f4 paste_from_buffer b

cursor_shape beam
cursor_beam_thickness 1.8

mouse_hide_wait 3.0
detect_urls yes

repaint_delay 10
input_delay 3
sync_to_monitor yes

map ctrl+shift+z toggle_layout stack
tab_bar_style powerline

inactive_tab_background #e06c75
active_tab_background #98c379
inactive_tab_foreground #000000
tab_bar_margin_color black

map ctrl+shift+enter new_window_with_cwd
map ctrl+shift+t new_tab_with_cwd

background_opacity 0.95

shell zsh
```

```zsh
# color.ini no escribas esta linea
cursor_shape          Underline
cursor_underline_thickness 1
window_padding_width  20

# Special
foreground #a9b1d6
background #1a1b26

# Black
color0 #414868
color8 #414868

# Red
color1 #f7768e
color9 #f7768e

# Green
color2  #73daca
color10 #73daca

# Yellow
color3  #e0af68
color11 #e0af68

# Blue
color4  #7aa2f7
color12 #7aa2f7

# Magenta
color5  #bb9af7
color13 #bb9af7

# Cyan
color6  #7dcfff
color14 #7dcfff

# White
color7  #c0caf5
color15 #c0caf5

# Cursor
cursor #c0caf5
cursor_text_color #1a1b26

# Selection highlight
selection_foreground #7aa2f7
selection_background #28344a
```

- Y luego cerramos con ctrl + q  y luego abrimos una nueva terminal para ver los cambios, no es necesario apretar ctrl + shift + r 
- Ahora tenemos que configurar zsh para el usuario root 
- Ahora ingresamos al usuario root e ingresamos el siguiente comando y luego cerramos la terminal para que se apliquen los cambios
```zsh
apt install zsh-autosuggestions zsh-syntax-highlighting zsh-autocomplete
```

- Ahora abrimos una ventana de firefox con ctrl + shift + f y buscamos kiity github y en el apartado de realeases clickeamos en la ultima version Linux amd64 binary bundle y la descargamos
- Luego ingresamos al usuario root
```zsh
sudo su
cd /opt/
mkdir kitty
cd kitty
mv /home/ressc/Downloads/kitty-0.28.1-x86_64.txz .
7z x kitty-0.28.1-x86_64.txz
rm kitty-0.28.1-x86_64.txz
tar -xf kitty-0.28.1-x86_64.tar
rm kitty-0.28.1-x86_64.tar
cd /bin/
./kitty --version
apt remove kitty
pwd # Copiamos la ruta
nano /home/ressc/.config/sxhkd/sxhkdrc # configuramos para que se abra la kitty en el proximo codigo
```

```zsh
# Terminal emulator
super + Return
	/opt/kitty/bin/kitty
```

- Luego cerramos todo con super + q y apretamos super + escape o super + shift + r
- Ahora tenemos zoom, abrimos una terminal y apretamos ctrl + shift + enter, y apretamos ctrl + shift + z y se realiza un zoom a la pantalla en la que estamos
- Ahora tenemos diferentes clip board para copiar y pegar
	- f1 -> copiar 
	- f2 -> pegar el f1
	- f3 -> copiar
	- f4 -> pegar el f3
- Con ctrl + shift + t, abirmos un nuevo entorno y con ctrl + shift y las flechas para moverme entre ellos

- Luego configuramos kitty en el usuario root
- Ingresamos a root con sudo su 
```zsh
cd /root/.config/kitty/
cp /home/ressc/.config/kitty/* .
/opt/kitty/bin/kitty # y veremos que se abre bien kitty en el usuario root
# Aparecera zsh compinit pero lo cierras despues veremos como solucionarlo
```

- Ahora vamos a establecer el fondo de pantalla aqui podemos elegir el que queramos
- Nos descargamos cualquier imagen que decee y creamos una carpeta en el escritorio con su nombre de usuario y adentro de ella una que se llame Fondos y ahi dejamos nuestra imagen
- Instalamos imagemagick para previsualizar la imagen por consola
```zsh
sudo apt install imagemagick
kitty +kitten icat fondo.png # asi podras visualizar tu imagen en consola
sudo apt install feh # Sirve para agregar nuestro fondo de pantalla
feh --bg-fill fondo.png
nano /home/ressc/.config/bspwm/bspwmrc # e ingresamos la linea de arriba al final agregando & para que se ejecute en segundo plano
```

```zsh
feh --bg-fill /home/ressc/Downloads/ressc/Fondos/fondo.png &
```


#### Despliegue de la Polybar

- Nos vamos a la carpeta de descargas y clonamos el siguiente repositorio y seguimos con los pasos se describen
```zsh
git clone https://github.com/VaughnValle/blue-sky.git
cd blue-sky
cd polybar
cp -r * ~/.config/polybar
echo '~/.config/polybar/./launch.sh' >> ~/.config/bspwm/bspwmrc
cd /fonts/
sudo cp * /usr/share/fonts/truetype
fc-cache -v # Esto es para que se apliquen los cambios
```

- Luego apretamos super + shift + r para que se apliquen los cambios

#### Configurando los bordeados, las sombras y los difuminados con Picom

- Creamos la carpeta picom en .config y dentro de ella el archivo picom.conf
```zsh
##############################################################################
#                                  CORNERS                                   #
##############################################################################
# requires: https://github.com/sdhand/compton
corner-radius = 20;
rounded-corners-exclude = [
  #"window_type = 'normal'",
  #"class_g = 'firefox'",
];

round-borders = 20;
round-borders-exclude = [
  #"class_g = 'TelegramDesktop'",
];

# Specify a list of border width rules, in the format `PIXELS:PATTERN`, 
# Note we don't make any guarantee about possible conflicts with the
# border_width set by the window manager.
#
# example:
#    round-borders-rule = [ "2:class_g = 'URxvt'" ];
#
round-borders-rule = [];

##############################################################################
#                                  SHADOWS                                   #
##############################################################################

# Enabled client-side shadows on windows. Note desktop windows 
# (windows with '_NET_WM_WINDOW_TYPE_DESKTOP') never get shadow, 
# unless explicitly requested using the wintypes option.
#
shadow = true

# The blur radius for shadows, in pixels. (defaults to 12)
shadow-radius = 15

# The opacity of shadows. (0.0 - 1.0, defaults to 0.75)
shadow-opacity = .5

# The left offset for shadows, in pixels. (defaults to -15)
shadow-offset-x = -15

# The top offset for shadows, in pixels. (defaults to -15)
shadow-offset-y = -15

# Avoid drawing shadows on dock/panel windows. This option is deprecated,
# you should use the *wintypes* option in your config file instead.
#
# no-dock-shadow = false

# Don't draw shadows on drag-and-drop windows. This option is deprecated, 
# you should use the *wintypes* option in your config file instead.
#
# no-dnd-shadow = false

# Red color value of shadow (0.0 - 1.0, defaults to 0).
# shadow-red = .18

# Green color value of shadow (0.0 - 1.0, defaults to 0).
# shadow-green = .19

# Blue color value of shadow (0.0 - 1.0, defaults to 0).
# shadow-blue = .20

# Do not paint shadows on shaped windows. Note shaped windows 
# here means windows setting its shape through X Shape extension. 
# Those using ARGB background is beyond our control. 
# Deprecated, use 
#   shadow-exclude = 'bounding_shaped'
# or 
#   shadow-exclude = 'bounding_shaped && !rounded_corners'
# instead.
#
# shadow-ignore-shaped = ''

# Specify a list of conditions of windows that should have no shadow.
#
# examples:
#   shadow-exclude = "n:e:Notification";
#
# shadow-exclude = []
shadow-exclude = [
    "class_g = 'firefox' && argb"
];

# Specify a X geometry that describes the region in which shadow should not
# be painted in, such as a dock window region. Use 
#    shadow-exclude-reg = "x10+0+0"
# for example, if the 10 pixels on the bottom of the screen should not have shadows painted on.
#
# shadow-exclude-reg = "" 

# Crop shadow of a window fully on a particular Xinerama screen to the screen.
# xinerama-shadow-crop = false

##############################################################################
#                                  FADING                                    #
##############################################################################

# Fade windows in/out when opening/closing and when opacity changes,
#  unless no-fading-openclose is used.
#fading = true

# Opacity change between steps while fading in. (0.01 - 1.0, defaults to 0.028)
# fade-in-step = 0.028
fade-in-step = 0.01;

# Opacity change between steps while fading out. (0.01 - 1.0, defaults to 0.03)
# fade-out-step = 0.03
fade-out-step = 0.01;

# The time between steps in fade step, in milliseconds. (> 0, defaults to 10)
# fade-delta = 10

# Specify a list of conditions of windows that should not be faded.
# fade-exclude = []

# Do not fade on window open/close.
# no-fading-openclose = false

# Do not fade destroyed ARGB windows with WM frame. Workaround of bugs in Openbox, Fluxbox, etc.
# no-fading-destroyed-argb = false

##############################################################################
#                                   OPACITY                                  #
##############################################################################

# Opacity of inactive windows. (0.1 - 1.0, defaults to 1.0)
inactive-opacity = 1.0

# Opacity of window titlebars and borders. (0.1 - 1.0, disabled by default)
frame-opacity = 1.0

# Default opacity for dropdown menus and popup menus. (0.0 - 1.0, defaults to 1.0)
opacity = 1.0

# Let inactive opacity set by -i override the '_NET_WM_OPACITY' values of windows.
# inactive-opacity-override = true
inactive-opacity-override = false;

# Default opacity for active windows. (0.0 - 1.0, defaults to 1.0)
active-opacity = 1.0

# Dim inactive windows. (0.0 - 1.0, defaults to 0.0)
# inactive-dim = 0.0

# Specify a list of conditions of windows that should always be considered focused.
# focus-exclude = []
focus-exclude = [ "class_g = 'Cairo-clock'" ];

# Use fixed inactive dim value, instead of adjusting according to window opacity.
# inactive-dim-fixed = 1.0

# Specify a list of opacity rules, in the format `PERCENT:PATTERN`, 
# like `50:name *= "Firefox"`. picom-trans is recommended over this. 
# Note we don't make any guarantee about possible conflicts with other 
# programs that set '_NET_WM_WINDOW_OPACITY' on frame or client windows.
# example:
#    opacity-rule = [ "80:class_g = 'URxvt'" ];
#
# opacity-rule = []

# opacity-rule = [ "98:class_g = 'Polybar'" ]

##############################################################################
#                                    BLUR                                    #
##############################################################################

# Parameters for background blurring, see the *BLUR* section for more information.
blur-method = "dual_kawase"
blur-size = 2
blur-strength = 3

# Blur background of semi-transparent / ARGB windows. 
# Bad in performance, with driver-dependent behavior. 
# The name of the switch may change without prior notifications.
#
blur-background = true

# Blur background of windows when the window frame is not opaque. 
# Implies:
#    blur-background 
# Bad in performance, with driver-dependent behavior. The name may change.
#
#blur-background-frame = false


# Use fixed blur strength rather than adjusting according to window opacity.
#blur-background-fixed = false


# Specify the blur convolution kernel, with the following format:
# example:
#   blur-kern = "5,5,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1";
#
# blur-kern = ''
# blur-kern = "3x3box";

# Exclude conditions for background blur.
# blur-background-exclude = []
#blur-background-exclude = [
#    "! name~=''",
#    "name *= 'slop'",
#    "window_type = 'dock'",
#    "window_type = 'desktop'",
#    "_GTK_FRAME_EXTENTS@:c"
#];

##############################################################################
#                                    GENERAL                                 #
##############################################################################

# Daemonize process. Fork to background after initialization. Causes issues with certain (badly-written) drivers.
# daemon = false

# Specify the backend to use: `xrender`, `glx`, or `xr_glx_hybrid`.
# `xrender` is the default one.
#
# backend = 'glx'
backend = "glx";

# Enable/disable VSync.
# vsync = false
vsync = false

# Enable remote control via D-Bus. See the *D-BUS API* section below for more details.
# dbus = false

# Try to detect WM windows (a non-override-redirect window with no 
# child that has 'WM_STATE') and mark them as active.
#
# mark-wmwin-focused = false
mark-wmwin-focused = true;

# Mark override-redirect windows that doesn't have a child window with 'WM_STATE' focused.
# mark-ovredir-focused = false
mark-ovredir-focused = true;

# Try to detect windows with rounded corners and don't consider them 
# shaped windows. The accuracy is not very high, unfortunately.
#
# detect-rounded-corners = false
detect-rounded-corners = true;

# Detect '_NET_WM_OPACITY' on client windows, useful for window managers
# not passing '_NET_WM_OPACITY' of client windows to frame windows.
#
# detect-client-opacity = false
detect-client-opacity = true;

# Specify refresh rate of the screen. If not specified or 0, picom will 
# try detecting this with X RandR extension.
#
# refresh-rate = 60
refresh-rate = 0

# Limit picom to repaint at most once every 1 / 'refresh_rate' second to 
# boost performance. This should not be used with 
#   vsync drm/opengl/opengl-oml
# as they essentially does sw-opti's job already, 
# unless you wish to specify a lower refresh rate than the actual value.
#
# sw-opti = 

# Use EWMH '_NET_ACTIVE_WINDOW' to determine currently focused window, 
# rather than listening to 'FocusIn'/'FocusOut' event. Might have more accuracy, 
# provided that the WM supports it.
#
# use-ewmh-active-win = false

# Unredirect all windows if a full-screen opaque window is detected, 
# to maximize performance for full-screen windows. Known to cause flickering 
# when redirecting/unredirecting windows.
#
# unredir-if-possible = false

# Delay before unredirecting the window, in milliseconds. Defaults to 0.
# unredir-if-possible-delay = 0

# Conditions of windows that shouldn't be considered full-screen for unredirecting screen.
# unredir-if-possible-exclude = []

# Use 'WM_TRANSIENT_FOR' to group windows, and consider windows 
# in the same group focused at the same time.
#
# detect-transient = false
detect-transient = true

# Use 'WM_CLIENT_LEADER' to group windows, and consider windows in the same 
# group focused at the same time. 'WM_TRANSIENT_FOR' has higher priority if 
# detect-transient is enabled, too.
#
# detect-client-leader = false
detect-client-leader = true

# Resize damaged region by a specific number of pixels. 
# A positive value enlarges it while a negative one shrinks it. 
# If the value is positive, those additional pixels will not be actually painted 
# to screen, only used in blur calculation, and such. (Due to technical limitations, 
# with use-damage, those pixels will still be incorrectly painted to screen.) 
# Primarily used to fix the line corruption issues of blur, 
# in which case you should use the blur radius value here 
# (e.g. with a 3x3 kernel, you should use `--resize-damage 1`, 
# with a 5x5 one you use `--resize-damage 2`, and so on). 
# May or may not work with *--glx-no-stencil*. Shrinking doesn't function correctly.
#
# resize-damage = 1

# Specify a list of conditions of windows that should be painted with inverted color. 
# Resource-hogging, and is not well tested.
#
# invert-color-include = []

# GLX backend: Avoid using stencil buffer, useful if you don't have a stencil buffer. 
# Might cause incorrect opacity when rendering transparent content (but never 
# practically happened) and may not work with blur-background. 
# My tests show a 15% performance boost. Recommended.
#
# glx-no-stencil = false

# GLX backend: Avoid rebinding pixmap on window damage. 
# Probably could improve performance on rapid window content changes, 
# but is known to break things on some drivers (LLVMpipe, xf86-video-intel, etc.).
# Recommended if it works.
#
# glx-no-rebind-pixmap = false

# Disable the use of damage information. 
# This cause the whole screen to be redrawn everytime, instead of the part of the screen
# has actually changed. Potentially degrades the performance, but might fix some artifacts.
# The opposing option is use-damage
#
# no-use-damage = false
use-damage = false

# Use X Sync fence to sync clients' draw calls, to make sure all draw 
# calls are finished before picom starts drawing. Needed on nvidia-drivers 
# with GLX backend for some users.
#
# xrender-sync-fence = false

# GLX backend: Use specified GLSL fragment shader for rendering window contents. 
# See `compton-default-fshader-win.glsl` and `compton-fake-transparency-fshader-win.glsl` 
# in the source tree for examples.
#
# glx-fshader-win = ''

# Force all windows to be painted with blending. Useful if you 
# have a glx-fshader-win that could turn opaque pixels transparent.
#
# force-win-blend = false

# Do not use EWMH to detect fullscreen windows. 
# Reverts to checking if a window is fullscreen based only on its size and coordinates.
#
# no-ewmh-fullscreen = false

# Dimming bright windows so their brightness doesn't exceed this set value. 
# Brightness of a window is estimated by averaging all pixels in the window, 
# so this could comes with a performance hit. 
# Setting this to 1.0 disables this behaviour. Requires --use-damage to be disabled. (default: 1.0)
#
# max-brightness = 1.0

# Make transparent windows clip other windows like non-transparent windows do,
# instead of blending on top of them.
#
# transparent-clipping = false

# Set the log level. Possible values are:
#  "trace", "debug", "info", "warn", "error"
# in increasing level of importance. Case doesn't matter. 
# If using the "TRACE" log level, it's better to log into a file 
# using *--log-file*, since it can generate a huge stream of logs.
#
# log-level = "debug"
log-level = "warn";

# Set the log file.
# If *--log-file* is never specified, logs will be written to stderr. 
# Otherwise, logs will to written to the given file, though some of the early 
# logs might still be written to the stderr. 
# When setting this option from the config file, it is recommended to use an absolute path.
#
# log-file = '/path/to/your/log/file'

# Show all X errors (for debugging)
# show-all-xerrors = false

# Write process ID to a file.
# write-pid-path = '/path/to/your/log/file'

# Window type settings
# 
# 'WINDOW_TYPE' is one of the 15 window types defined in EWMH standard: 
#     "unknown", "desktop", "dock", "toolbar", "menu", "utility", 
#     "splash", "dialog", "normal", "dropdown_menu", "popup_menu", 
#     "tooltip", "notification", "combo", and "dnd".
# 
# Following per window-type options are available: ::
# 
#   fade, shadow:::
#     Controls window-type-specific shadow and fade settings.
# 
#   opacity:::
#     Controls default opacity of the window type.
# 
#   focus:::
#     Controls whether the window of this type is to be always considered focused. 
#     (By default, all window types except "normal" and "dialog" has this on.)
# 
#   full-shadow:::
#     Controls whether shadow is drawn under the parts of the window that you 
#     normally won't be able to see. Useful when the window has parts of it 
#     transparent, and you want shadows in those areas.
# 
#   redir-ignore:::
#     Controls whether this type of windows should cause screen to become 
#     redirected again after been unredirected. If you have unredir-if-possible
#     set, and doesn't want certain window to cause unnecessary screen redirection, 
#     you can set this to `true`.
#
wintypes:
{
  tooltip = { fade = true; shadow = true; shadow-radius = 0; shadow-opacity = 1.0; shadow-offset-x = -20; shadow-offset-y = -20; opacity = 0.8; full-shadow = true; }; 
  dnd = { shadow = false; }
  dropdown_menu = { shadow = false; };
  popup_menu    = { shadow = false; };
  utility       = { shadow = false; };
}
```

- Luego cargamos el picom para que se apliquen los cambios en .config/picom
```zsh
picom &
```

- Modificamos .config/bspwm/bspwmrc
```zsh
nano ~/.config/bspwm/bspwmrc
```

- E ingresamos al final 
```
bspc config border_width 0
```

- Presionamos super + shift + r para que se apliquen los cambios

#### Configurando la ZSH e instalando Powerlevel10k
- Nos clonamos la instalacion manual de Powerlevel10k y configuramos al gusto el apartado visual, es intuitivo. Usuario normal no es necesario realizarlo con root
```zsh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```
- Y ingresamos zsh para proceder con la instalacion
- Podemos elminar el echo de inicio de terminal en .zshrc
- Ahora editamos archivo .p10k.zsh
- Comentaremos todo lo de adentro de la funcion typeset -g POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS =( ....
- En el mismo archivo en typeset -g POWERLEVEL9K_LEFT_PROMPT_ELEMENTS =(... . ingresamos los siguiente
```zsh
context
command_execution_time
status
```
- Y ahora para el usuario root 
```zsh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```

- ingresamos zsh y apretamos ctrl + c apenas salga el primer mensake despues ingresamos el comando compaudit y volvemos a apretar control + c
- En la linea there arre isecure file aparece una ruta, la copiamos y escribimos lo siguiente ya que deberia de dirigir a el usuario root y no al local
```zsh
chown root:root /usr/local/share/zsh/site-functions/_bspc
```

- Cerramos la terminal y volvemos a abrir otra e ingresamos zsh para proceder con la configuracion, la misma que realizamos para el usuario local
- Cerramos la terminal e ingresamos lo siguiente en el usuario root ya que no se visualiza la configuracion
```zsh
cat /etc/passwd | grep -E "^ressc|^root" # Visualizamos 
usermood --shell /usr/bin/zsh root
usermood --shell /usr/bin/zsh ressc
```

- Ahora crearemos un link simbolico de la zsh para que lo que cambio en la zshrc del usuario ressc se aplique tambien para el usuario root
- ingresamos con sudo su, y colocamos cd para ir a la raiz e introducimos el siguiente comando
```zsh
ln -s -f /home/ressc/.zshrc .zshrc
ls -la # Veremos que se creo correctamente
```
- Cerramos la terminal y volvemos a abrir y configuramos en el usuario root el archivo .zshrc y cambianos la siguiente linia por 
```zsh
source ~/powerlevel10k.zsh-theme # Cambiar esta por la linea de abajo esta casi al final del archivo
source /home/ress/powerlevel10k.zsh-theme
```

- Ahora ajustaremos la visual del usuario root tal cual se realizo para el usuario no privilegiado 
```zsh
nano ~/.p10k.zsh
```

- Y realizamos lo mismo de comentar e ingresar las atributos de la ezquierda
- Ahora colocaremos un icono en with root@parrot para el tema visual
- Colocaremos una llama e ingresamos a nano ~/.p10k.zsh y filtramos por ROOT_TEMPLATE e eliminamos y dejamos el texto vacio de # costom icon y en root_template eliminamos el texto y copiamos algun icono de llama que deceamos de nerdfonts 
- Ahora ajustaremos el history para que se vea bien, abrimos .zshrc en el usaurio no privilegiado
- Filtramos por history y precionamos ctrl +w para ver mas resultado y en el utimo ingresamos los siguiente por lo que existia
```zsh
setopt appendhistory # cambiamos esta linea por
setopt histignorealldups sharehistory
```

- Luego en el mismo archivo comentamos el plugind del autocompletado el segundo if de fish like syntax highlighting
-  Y con echo '' > ~/.zsh_history vaciamos el historial correctamente
- Ahora retocaremos las negritas que se visualizan en el layout 
- Ingresamos a .p10k.zsh  y cambiamos el boleano de DIR_ANCHOR_BOLD de true a false
- Y realizamos lo mismo para el usuario root
- Y ahora por ultimo en la parte del autocompletado en el archivo ~/.zshrc y abajo de lo que comentamos del autocompletado eliminamos lo comentado e ingresamos lo siguiente
```zsh
# Use modern completion system
autoload -Uz compinit
compinit
 
zstyle ':completion:*' auto-description 'specify: %d'
zstyle ':completion:*' completer _expand _complete _correct _approximate
zstyle ':completion:*' format 'Completing %d'
zstyle ':completion:*' group-name ''
zstyle ':completion:*' menu select=2
eval "$(dircolors -b)"
zstyle ':completion:*:default' list-colors ${(s.:.)LS_COLORS}
zstyle ':completion:*' list-colors ''
zstyle ':completion:*' list-prompt %SAt %p: Hit TAB for more, or the character to insert%s
zstyle ':completion:*' matcher-list '' 'm:{a-z}={A-Z}' 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=* l:|=*'
zstyle ':completion:*' menu select=long
zstyle ':completion:*' select-prompt %SScrolling active: current selection at %p%s
zstyle ':completion:*' use-compctl false
zstyle ':completion:*' verbose true
 
zstyle ':completion:*:*:kill:*:processes' list-colors '=(#b) #([0-9]#)*=0=01;31'
zstyle ':completion:*:kill:*' command 'ps -u $USER -o pid,%cpu,tty,cputime,cmd'
```

#### Definiendo las propiedades de consola e instalando batcat y lsd
- Ahora instalaremos un cat mas moderno y un ls
- Nos metemos al repositorio de github al apartado de release en la utima version de bat y descrgamos el archivo .deb de amd
-  Nos vamos a la carpeta de descargas y ingresamos lo siguiente
```zsh
dpkg -i bat_0.23.0_amd64.deb
bat /etc/hosts # Visualizamos de mejor manera
```
- Y ahora lo mismo para el repositorio de lsd y realizamos el mismo paso anterior pero con el archivo correspondiente
- Instalamos con el usuario root lacate para localizar archivos del sistema rapidamente
- Realizamos un updatedb para sincronizar todos los arichivos de nuestro sistema y aparese un permision denied y se soluciona ingresando el siguiente comando para que en un futuro que sincronicemos la data no aparezca el error
```zsh
umount /run/user/1000/doc
```
- Y al realizar updatedb nuevamente no aparecera nada
- Para utilizar locate podemos realizar locate kitty.conf y apareceran todas las rutas donde se encuentre kitty.conf
- Ahora configuramos las negritas  en .zshrc 
- Por consola realizamos un echo $LS_COLORS copiamos lo que imprime y escribimos al final del todo de .zshrc export LS_COLORS = "el text que se copio" pero hay qu eliminar todos lo 01; del contenido y guardamos para que se apliquen los cambios
-  Ahora crearemos una serie de alias en .zshrc
```zsh
# Custom Aliases
# -----------------------------------------------
# bat
alias cat='bat'
alias catn='bat --style=plain'
alias catnp='bat --style=plain --paging=never'
 
# ls
alias ll='lsd -lh --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias l='lsd --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'
```

- En la kitty con ctrl + alt seleccionamos en la kitty
- Al abrir burpsuite existe un error de java esto se soluciona agregando esta linea en .zshrc en la quinta linea
```zsh
# Fix the Java Problem
export _JAVA_AWT_WM_NONREPARENTING=1
```
- Y en .config/bspwm/bspwmrc arriba de bspc config ingresamos lo siguiente
```zsh
wmname LG3D &
```
- Si la resolucion se descofigura por alguna razon realizamos un
```zsh
xrandr --output Virtual1 --mode 1920x1080
```



#### Configurando la Polybar

 - Ahora vamos a crear nuestros propios modulos en la barra
 - Por terminal podemos apretar ctrl + shift + u para buscar por exagecimal los iconos
 - Primero debemos de crear la carpeta bin en .config del usuario no privilegiado
 - Posteriormente nos vamos al apartado .config/polybar y tenemos que crear dos modulos en current.ini, abajo de [bar/secundary] copiamos los siguientes
```zsh
[bar/ethernet_bar]
inherit = bar/main
width = 10%
height = 40
offset-x = 3.8%
offset-y = 15
background = ${color.bg}
foreground = ${color.white}
bottom = false
padding = 1
;padding-top = 2
module-margin-left = 0
module-margin-right = 0
;modules-left = date sep mpd
modules-center = ethernet_status
wm-restack = bspwm

[bar/vpn_bar]
inherit = bar/main
width = 11%
height = 40
offset-x = 14.05%
offset-y = 15
background = ${color.bg}
foreground = ${color.white}
bottom = false
padding = 1
;padding-top = 2
module-margin-left = 0
module-margin-right = 0
;modules-left = date sep mpd
modules-center = vpn_status
wm-restack = bspwm
```

- Y mas abajo en el mismo archivo en modules abajo del [module/date]  copiamos los sguientes modulos
```zsh
[module/ethernet_status]
type = custom/script
interval = 2
exec = ~/.config/bin/ethernet_status.sh

[module/vpn_status]
type = custom/script
interval = 2
exec = ~/.config/bin/vpnstatus.sh
```

- Guardamos y editamos el archivo launch.sh comentamos y escribimos los dos launch
```zsh
## Left bar
polybar log -c ~/.config/polybar/current.ini &
## polybar secondary -c ~/.config/polybar/current.ini &
polybar ethernet_bar -c ~/.config/polybar/current.ini &
polybar vpn_bar -c ~/.config/polybar/current.ini &
```

- Ahora no movemos a la raiz y ingresamos a bin y creamos los dor archivos ethernetstatus.sh y vpn_status.sh
- Le damos permisos de ejecucion con el siguiente comando a cada uno
```zsh
chmod +x [nombre del archivo]
# Eso es para cada uno
```

- Despues copiamos lo siguiente en cada uno
- nano vpn_status.sh
```zsh
#!/bin/sh
 
IFACE=$(/usr/sbin/ifconfig | grep tun0 | awk '{print $1}' | tr -d ':')
 
if [ "$IFACE" = "tun0" ]; then
    echo "%{F#1bbf3e}ICONO %{F#ffffff}$(/usr/sbin/ifconfig tun0 | grep "inet " | awk '{print $2}')%{u-}"
else
    echo "%{F#1bbf3e}ICONO %{u-} Disconnected"
fi
```
- nano ethernet_status.sh
```zsh
#!/bin/sh
 
echo "%{F#2495e7}ICONO %{F#ffffff}$(/usr/sbin/ifconfig ens33 | grep "inet " | awk '{print $2}')%{u-}"
```

- Guardamos cada archivo y precionamos super + shift + r para visualizar los cambios
- Ahora puedes descargarte la vpn de hackthebox y ejecutarla con sudo openvpn y el archivo que descargas de la misma pagina

#### Creando nuevos módulos en la Polybar
- Vamos a realizar los mismos pasos que realizamos anteriormente
- En .config/polybar/ editamos el archivo current.ini y realzamos esto
```zsh
[bar/victim_target]
inherit = bar/main
width = 17%
height = 40
offset-x = 79.6%
offset-y = 15
background = ${color.bg}
foreground = ${color.white}
bottom = false
padding = 1
;padding-top = 2
module-margin-left = 0
module-margin-right = 0
;modules-left = date sep mpd
modules-center = target_to_hack
wm-restack = bspwm


# Y en el apartado de modulos lo siguiente
[module/target_to_hack]
type = custom/script
interval = 2
exec = ~/.config/bin/target_to_hack.sh

```

- Posteriormente en .config/bin tenemos que tener un archivo taget para que se pueda modificar creamos el archivo target_to_hack.sh ingresamos lo siguiente. recordar que se le debe de dar permisos de ejecucion con chmod +x [nombre del archivo]
- Remplazar ICON con el icono a eleccion en nuestro caso buscamos uno por nerdfonts con target
```zsh
#!/bin/bash
 
ip_address=$(/bin/cat /home/ressc/.config/bin/target | awk '{print $1}')
machine_name=$(/bin/cat /home/ressc/.config/bin/target | awk '{print $2}')
 
if [ $ip_address ] && [ $machine_name ]; then
    echo "%{F#e51d0b}ICON %{F#ffffff}$ip_address%{u-} - $machine_name"
else
    echo "%{F#e51d0b}ICON %{u-}%{F#ffffff} No target"
fi

```

- Despues editamos en la raiz .zshrc para agregar dos funciones que nos permitiran automatizar el proceso de asignar un target y realizar un clear. Todo esto lo copiamos debajos de las funciones que ya se encuentran creadas
```zsh
function settarget(){
    ip_address=$1
    machine_name=$2
    echo "$ip_address $machine_name" > /home/ressc/.config/bin/target
}

function cleartarget(){
    echo '' > /home/ressc/.config/bin/target
}
```

- Realizamos ctrl + shift + r y verificamos los cambios
- Cerramos la consoloa y ahora con settarget + "nombre +. ip" podemos definir rapidamente
- Con clear target borramos todo
- Ahora editaremos mejor para saber en que espacio de trabajo estamos
- En el archivo workspace.ini en la ruta .config/polybar cambiamos los colores para una mejor visualizacion
```zsh
label-active-foreground = ${color.red}
label-active-background = ${color.bg}

label-occupied = "%icon% "
label-occupied-foreground = ${color.yellow}
label-occupied-background = ${color.bg}
```



#### Instalando plugins en la ZSH y configurando NVChad con Neovim e i3lock-fancy
- Ahora vamos a installar un plugin de zsh con wget
- Nos dirigimos en usuario root a la carpeta /usr/share y creamos la carpeta zsh-sudo  y escribimos esto para que el propietario sea el usuario no privilegiado
```zsh
chown ressc:ressc zsh-sudo
```
- Ingresamos a la carpeta y realizamos un wget de esto:
```zsh
wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/plugins/sudo/sudo.plugin.zsh
# y ahora le damos permisos de ejecucion
chmod +x [nombre del archivo]
```
- Copiamos la ruta completa del archivo con pwd mas el archivo
- Nos dirigimos a .zshrc
- Y copiamos este estracto abajo de algun otro plugin que tengamos instalado
```zsh
if [ -f /usr/share/zsh-sudo/sudo.plugin.zsh ]; then 
    source /usr/share/zsh-sudo/sudo.plugin.zsh
fi
```
- Guardamos y al abrir otra pestalla de terminal y escribimos whoami y presionamos esc esc se cambia automaticamente 
- Y asi es como se instala cualquier plugin que queramos
- Ahora iremos con el neovin que es mucho mejor que nano
- Primero ingresamos a root y e instalamos npm 
```zsh
apt install npm
```

- Primero eliminamos .config/nvim todo esto con el usuario no provilegiado
```zsh
rm -r .config/nvim
```
- Ahora ejecutamos esto en la carpeta raiz 
```zsh
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1
```
- Despues nos vamos al repositorio de nvim y nos descargamos la version mas reciente nvim-linux.tar.gz
- Nos vamos al usuario root y nos desplazamos lo descargado al directorio de trabajo opt
```zsg
cd /opt/
mv /home/ressc/Download/nvim-linux64.tar.gz .
tar -xf nvim-linux64.tar.gz
rm nvim-linux64.tar.gz
cd nvim-linux64
```
- Ahora si lo que esta al interior de la carpeta lo ejecutamos como usuario no privilegiado 
```zsh
cd /opt/nvim-linux/bin
./nvim
```

- Ahora ingresamos no y instalara todo lo necesario
- Luego cuando aparezca NOTES ingresamos :q!
- Ahora nos borramos nvim con root 
```zsh
apt remove neovim
```

- Ahora configuramos el PATH para que busque la ruta del binario de NVIM
```zsh
nano .zshrc
# ingresar por algun lado del path esto
/opt/nvim-linux64/bin # recordar que tienen que estar separados por : y es la ruta completa donde se encuetra el binario
```

- Si quieres x eliminar el la sugerencia en nvim te vas al archivo init.lua de la ruta .config/nvim/lua/plugins 
- Y eliminas desde la linea 153 a la 199 donde dice --load luasnips
- Al escribir :NvimTreeToggle podrmos acceder similar a vscode a la estructar de carpetas del directorio
- Ahora configuraremos el theme del rofi personalizado, si bien puedes configurando escribiendo en consola rofi-theme-selector 
- Si nos vamos al archivo previamente descargado en apartados anteriores en descargas/blue-shy 
- Creamos en .config lo siguiente
```zsh
	mkdir -p ~/.config/rofi/theme
	cp nord.rasi ~/.config/rofi/theme
	# Y realizamos el rofi-theme-selector
	rofi-theme-selector
```

- Seleccionamos al final del todo nord y precionamos alt + a para que se apliquen los cambios
- Para el tema de fotos y demas descargaremos flameshot y ranger
```zsh
sudo su
apt install flameshot ranger
```
- ranger sirve para una visualizacion de archivos ingresando por consola
- flameshot es para sacar una captura de pantalla
```zsh
flameshot gui
```

- Puedes crear un shorcut en .config/sxhkd/sxhkdrc y agregamos el shurtcut
```zsh
super + shift + g
	/usr/bin/flameshot gui
```
- Cerramos sesion y podremos utilizar el comando, tambien puedes presionar super + shift + r para cargar
- Para el bloqueo de pantalla instalaremos i3lock con root
```zsh
sudo su
apt install i3lock
```
- Ahora clonamos el repositorio de i3lock-fansy en el directorio descargas 
```zsh
git clone https://github.com/meskarune/i3lock-fancy.git
cd i3lock-fancy
sudo make install
```

- Ahora podemos crear un shortcut para que se ejecute de la misma manera que el anterior, podemos asignar super + shift + x tarea para la casa
- Ahora configuramos nvim para el usuario root
```zsh
sudo su
cd 
whoami # aqui tiene que salir root
cd .config/nvim
cd ..
rm -rf nvim # verificar que estamos dentro de .config
cp -r /home/ressc/.config/nvim .
```
- Finalmente instalamos fzf para el usuario root y no provilegiado
```zsh
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```


#### Resumen Comandos ParrotOS
- super + enter -> Abrirmos consola
- super + q -> Cerramos la consola o la ventena que este activa
- super + 2 -> Para ir al segundo espacio de trabajo
- super + shift + 3 -> Si quiero mandar algo a un x espacio de trabajo
##### Preselectores
- ctrl + super + alt + [Flechas] -> Preselector
- ctrl + super + [Numeros] -> Ajusta el tamaño del preselector
- super + d -> rofi y lo podemos lanzar al preselector
- super + s -> Lo convertimos a ventana flotante
- super + click izq -> Podemos mover la ventana
- super + alt + [flechas] -> Ajustamos el tamaño a convenir
- super + click der -> Podemos realizar las mismas modificaciones anteriores
- super + t -> Deja la ventana flotante como estaba
- super + shift + [fleshas] -> Mover ventana que esta seleccionada
- ctrl + super + alt + spacio -> cancela el preselector
##### Kitty
- ctrl + shift + enter -> Abrir una nueva consola en la kitty
- ctrl + shift + w -> Cerrar la ventana de la kitty que se abrio previamente
- ctrl + [flechas] -> Te mueves entre ventanas de la misma kitty
- ctrl + shift + r -> Resize la ventana de la kitty
- ctrl + shift + t -> abre una nueva ventana dentro de la consola de la kitty
- ctrl + shift + alt + t -> configuramos el nombre de la ventana
- ctrl + shift + [flechas o punto o coma] -> Moverme entre ventanas 
- ctrl + shift + w -> cierro la ventana actual de la kitty
- ctrl + t -> desde la raiz podemos listar rapidamente los archivos
- ctrl + r -> Puedo moverme entre el historial
- alt + h -> Dentro de nvim podemos abrir una terminal en la kitty
##### Plugins
esc + esc -> Se aplica el usuario sudo
settarget "Test 10.10.10.101"
cleartarget
echo '' > ~/.zsh_history -> Cree una funcion llamada clearhistory
