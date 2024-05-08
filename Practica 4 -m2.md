# NetBSD

installar paquetes: ``pkgin install
``pkgin install mate | slim | xdm
``echo 'slim=YES' >> /etc/rc.conf
``cp /usr/pkg/share/examples/rc.d/slim /etc/rc.d/
``echo exec mate-session > /home/usuario/.xsession
Los paquetes instalados instalan su rc.d en /usr/pkg/share/examples/rc.d, hay que moverlos a /etc/rc.d para que /etc/rc.conf funcione (no se muy bien por que)
poner la variable de entorno PKG_RCD_SCRIPTS=yes hace que se instalen automaticamente en /etc/rc.d

### ports
instalar
```
wget http://ftp.netbsd.org/pub/pkgsrc/stable/pkgsrc.tar.gz
tar -xzf pkgsrc.tar.gz -C /usr/
```
 usar
```
cd /usr/pkgsrc/editors/vim/
make install
hacer con otro paquete
```
# Ubuntu

``apt install mate | slim
``echo exec mate--sesion > /home/usuario/.xsession
No consegui que slim funcione
instalar guest aditions
meter disco
``mount /dev/cdrom /mnt
``apt install build-essential dkms linux-headers-$(uname -r)
``sh ./VBoxLinuxAdditions.run
#### slim
No se por qué funcionó, pero hice 
`apt-get --purge slim
`apt-get install slim
`dpkg-reconfigure slim`
`reboot`