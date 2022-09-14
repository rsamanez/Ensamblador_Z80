# Curso de Ensamblador Z80
### Preparacion de una Maquina Virtual Linux Manjaro
```
Confoguracion del X11VNC para acceder remotamente:

x11vnc -storepasswd
cd .vnc
touch x11vnc.log
x11vnc -nap -wait 50 -noxdamage -rfbauth /home/[YOUR_USER]/.vnc/passwd -display :0 -nocursor -forever -o /home/[YOUr_USER]/.vnc/x11vnc.log -bg

Configurando  x11vnc como servicio

sudo nano /etc/systemd/system/x11vnc.service
```
contenido de /etc/systemd/system/x11vnc.service
```
[Unit]
Description=VNC Server for X11
Requires=display-manager.service
After=display-manager.service

[Service]
Type=forking
ExecStart=/usr/bin/x11vnc -nap -wait 50 -display :1  -rfbauth /home/[YOUR_USER]/.vnc/passwd -forever -o /home/[YOUR_USER]/.vnc/x11vnc.log -bg
Restart=on-failure
RestartSec=10
User=[YOUR_USER]

[Install]
WantedBy=multi-user.target
```
Activar el servicio
```
sudo systemctl start x11vnc.service
sudo systemctl enable x11vnc.service
```
### Instalacion del CPCTelera
```
sudo pacman -Sy make bison flex mono boost freeimage
git clone https://github.com/lronaldo/cpctelera.git -b development
cd cpctelera
./setup.sh
```
Dado que el terminal por default es zsh se debe agregar manualmente el path de los scipts de CPCtelera
```
nano ~/.zshrc
```
contenido a agregar al final del archivo: .zshrc
```
###CPCTELERA_START
##
## These lines configure CPCtelera in your system
##

export CPCT_PATH=/home/[YOUR_USER]/cpctelera/cpctelera
export PATH=${PATH}:/home/[YOUR_USER]/cpctelera/cpctelera/tools/scripts

###CPCTELERA_END 
```
### Instalacion de Retro Virtual Machine
```
sudo pacman -S snapd
sudo systemctl enable --now snapd.socket
sudo systemctl enable --now snapd.apparmor
sudo ln -s /var/lib/snapd/snap /snap
sudo snap install mmartinortiz-retro-virtual-machine
```
Configurar RVM en cpcTelera
```
cpct_mkproject gameOne
cd gameOne
make
cpct_rvm gameOne.dsk
```
Te pedira el PATH de RVM que instalaste, le pasas el siguiente PATH
```
/snap/mmartinortiz-retro-virtual-machine/current/usr/bin/RetroVirtualMachine
```
### Instalar WinAPE
```
sudo pacman -S wine winetricks wine-mono wine_gecko
```
Bajar WinApe de la web : https://www.winape.net.   
luego ejecutar el script de CPCtelera para configurar el PATH del winApe
```
cpct_winape gameOne.dsk
```
configurarle el PATH donde se encuentra el ejecutable WinApe.EXE, ejemplo
```
/home/[YOUR_USER]/winape/WinApe.exe
```





