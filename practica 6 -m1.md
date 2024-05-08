# DEVUAN
```
apt install whois (para mkpasswd)
vim create_user.sh
```
Script para crear usuarios con contraseña:
```
#!/bin/bash

for i in `seq -w 0 999`; do
	useradd -m --shell /bin/bash -p `mkpasswd --method=yescrypt qwerty$i` user$i;
done
```

### PAM
Modificar /etc/pam.d/login para bloquear root por cli y /etc/pam.d/lightdm para bloquer root por login grafico
```
auth required pam_succeed_if.so user != root quiet_success
```
Para que solo user444 user555 y user666 hagan su sin contraseña y usuario lo haga con contra:
```
groupadd wheel
groupadd topWheel
usermod -aG wheel user444
usermod -aG wheel user555
usermod -aG wheel user666
usermod -aG wheel usuario
usermod -aG topWheel user444
usermod -aG topWheel user555
usermod -aG topWheel user666
vim /etc/pam.d/su
	auth required pam_wheel.so use_uid
	auth sufficient pam_wheel.so use_uid trust group=topWheel
```

### Añadir fecha de caducidad
```
usermod -e 2023-04-08 user100
usermod -e 2023-04-08 user200
```

### Alias INSTALADORES 

añadir en /etc/sudoers (con visudo)
```
# Cmnd alias specification
Cmnd_Alias INSTALADORES = usr/bin/dpkg, /usr/bin/apt, /usr/bin/aptitude, /usr/bin/apt-get

#User privilege specification
user100 ALL=(root) INSTALADORES
user101 ALL=(root) INSTALADORES
user102 ALL=(root) INSTALADORES
```

Para que vaya hacer sudo apt, solo funcionara si estas en el alias
### Directorio Intercambio
```
groupadd intercambio
usermod -aG intercambio user008
usermod -aG intercambio user009

mkdir /var/INTERCAMBIO
chmod 770 /var/INTERCAMBIO
chown root:intercambio /var/INTERCAMBIO

gpasswd intercambio
	intercambio

para entrar al grupo como otro usr:
newgrp intercambio
```
Modificar /etc/gshadow y poner user007 en el segundo campo desde la derecha para permitirle cambiar la contraseña.
`intercambio:x:user007:user008,user009`
# SOLARIS
Script para crear usuarios:
```
#!/bin/bash

for i in `seq -w 0 999`; do
	useradd -m -s /bin/bash user$i;
done
```
Script para crear contraseña:
```
#!/bin/bash

for i in `seq -w 0 999`; do
	/export/home/usuario/passwd.sh user$i qwerty$i; 
	(passwd.sh es el script que te da el tio con permisos de ejecución.)
done
```
passwd.sh
```
#!/usr/bin/expect -f
if {$argc!=2} {
send_user "uso: $argv0 usuario password \n"
exit
}
set user [lindex $argv 0]
set password [lindex $argv 1]
spawn passwd $user
expect "New Password: "
send "$password\r"
expect "Re-enter new Password: "
send "$password\r"
send "\r"
expect eof
```

### SU roles
En solaris root es un rol, por lo que no hace falta bloquerarlo del login correcto, para darle el rol a usuarios concretos:
Para que solo usuario user444 user555 y user666 hagan su con contra:
```
usermod -R +root user444
usermod -R +root user666
usermod -R +root user555
usermod -R +root usuario
```

Para añadir fecha ya expirada modificar /etc/shadow, cuenta el número de días desde Jan 1, 1970. En este caso añadimos 19455 en el penultimo valor
```
user200:x:19849:::::19455:
```
### Packages role
```
roleadd -c "Administrar paquetes" -s /usr/bin/pfbash -m -K profiles="Software Installation" Instalador
usermod -R +Instalador user100
usermod -R +Instalador user101
usermod -R +Instalador user102
```
Para entrar en el rol y usarlo:
`su Instalador`, solo eso usuarios pueden hacerlo

### Rol intercambio
```
groupadd intercambio
usermod -G intercambio user008
usermod -G intercambio user009

mkdir /var/INTERCAMBIO
chmod 770 /var/INTERCAMBIO
chown root:intercambio /var/INTERCAMBIO
```
Para crear el perfil:
```
vim /etc/security/prof_attr
	intercambio:::Perfil para entrar en intecambio:
vim /etc/security/exec_attr
	intercambio:solaris:cmd:::/usr/sbin/*:gid=101

		Esto no va:
roladd -c "intercambio" -m intercambio
rolemod -P intercambio intercambio
passwd intercambio
	intercambio123
```
 Preguntar como permitir a otros usuarios entrar en la carpeta
# OPENBSD

Script para crear usuarios con contra:
```
#!/bin/bash

for i in `seq -w 0 999`; do
	useradd -m -p `encrypt qwerty$i` -k /etc/skel user$i;
done
```

Para bloquear root:
Borrar  en /etc/ttys las palabras secure  de las lineas tty
`echo exit > /root/.xsession`
### Añadir fecha de caducidad
```
usermod -e 2023-04-08 user100
usermod -e 2023-04-08 user200
```

### Login Class desegunda
Añadir y configurar la login class en /etc/login.conf:
```
desegunda:\
	:filesize=1M:\
	:login-backoff=3:\
	:minpasswordlen=13:\
	:maxproc=20:\
	:priority=20:\
```
Como tiene una bd, para que se aplique hacer:
`cap_mkdb /etc/login.conf`

añadir login class: `usermod -L desegunda user013`
añadir login class: `usermod -L desegunda user013`
Para verlo en /etc/master.passwd sale despues de los ids de usuario y grupo
