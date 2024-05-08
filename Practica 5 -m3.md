# FreeBSD
## Para crear la jail
Con el instalador de FreeBSD en el disco (disc1, no bootonly):
```
mkdir -p /usr/jail/jaulita
cd /usr/jail/jaulita
mount -t cd9660 /dev/cd0 /mnt
tar xvJf /media/mnt/usr/freebsd-dist/base.txz
tar xvJf /media/mnt/usr/freebsd-dist/ports.txz
```
Crear archivo /etc/jail.conf 
```
pruebajail {
	path = /usr/jail/JAULILLA;
	mount.devfs;
	host.hostname = jailcilla;
	ip4.addr = 10.0.2.25;
	interface = em0;
	exec.start = "/bin/sh /etc/rc";
	exec.stop = "/bin/sh /etc/rc.shutdown";
}
```
## Para empezar la jail:
`jail -c pruebajail`
o alternativamente:
`service jail start|stop <jailname>`

Para que se inicie en boot, en /etc/rc.conf `jail_enable="YES"`
`jls` para listar jails

Para ejecutar un comando desde el host en la jail:
`jexec -l <jailname> <comando>`
## Para entrar a la jail 
`jexec -u root <jailname>`
Una vez dentro hice:
```
passwd (y pones la contra del root)
adduser (para crear los usuarios)
```


