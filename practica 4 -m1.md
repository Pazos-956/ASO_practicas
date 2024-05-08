## Errores
[[ERRORES]]
# OpenBSD
Apagar máquina: ``halt -p
Actualizar máquina
``pkg_add -u

instalar firefox, icewm y mate
``pkg_add firefox| icewm | mate

hacer que usuario use el manejador de ventanas
`` echo exec icewm > $HOME/.xsession
para que esté mate cambiar por:
`` exec mate-session

### Instalar ports
Descargar comprimido y certificado
``cd /tmp
``ftp http://ftp.usa.openbsd.org/pub/OpenBSD/$(uname -r)/ports.tar.gz
``ftp http://ftp.usa.openbsd.org/pub/OpenBSD/$(uname -r)/SHA256.sig
(Verificar el comprimido)
`` signify -C -p /etc/signify/openbsd-74-base.pub -x SHA256.sig ports.tar.gz
``tar -zxvf ports.tar.gz -C /usr/

Para usar ports
desde /usr/ports
1. Buscar la carpeta del paquete que se desea instalar
2. Hacer un comando make

`` cd /usr/ports/www/links
``make install 
Para crear un paquete que se puede instalar con pkg_add:
`` cd /usr/ports/www/lynx
``make package 
``pkg_add lynx`` (preguntar, no se si se instala asi o de otra forma)

/etc/rc.conf.local tiene la configuración de la máquina y /etc/rc.conf las opciones posibles
