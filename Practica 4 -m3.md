## ERRORES
[[ERRORES]]
# Fedora
instalar guest aditions
``dnf install build-essential dkms linux-headers-$(uname -r)
fedora tiene un paquete llamada 'virtualbox-guest-additions' ya instalado, para actualizarlo: `dnf update virtualbox-guest-additions`

# FreeBSD
``pkg install virtualbox-ose-additions
en el archivo /etc/rc.conf meter las lineas:
``vboxguest_enable="YES"
``vboxservice_enable="YES"
meter en .xsession el `exec mate-ssesion`
```
pkg install xorg
pkg install mate
pkg install xdm
pkg install slim
pkg install lightdm
pkg install lightdm-gtk-greeter
```
en el archivo /etc/rc.conf meter:
```
moused_enable="YES"
dbus_enable="YES"
xdm_enable="YES" | slim_enable="yes" | lightdm_enable="YES"
```
`echo exec mate-session > .xsession`
### ports
instalar
```
pkg install portsnap
portsnap fetch
portsnap extract
```
usar
```
cd /usr/ports/editors/vim
make install (si a todo)
hacer lo mismo con otro paquete random
```
