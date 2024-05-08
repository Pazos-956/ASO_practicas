# UBUNTU SERVER
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
Modificar /etc/pam.d/login para bloquear root por cli y /etc/pam.d/slim para bloquer root por login grafico
```
auth required pam_succeed_if.so user != root quiet_success
```
Para que solo user444 user555 y user666 hagan sudo:
```
usermod -aG sudo user444
usermod -aG sudo user555
usermod -aG sudo user666
en visudo podemos ver que el grupo sudo es el unico que puede usarlo, por eso lo añadimos aqui
```

### Añadir fecha de caducidad
```
usermod -e 2023-04-08 user100
usermod -e 2023-04-08 user200
```

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

# NETBSD

Script para crear usuarios con contra:
```
#!/bin/bash

for i in `seq -w 0 999`; do
	useradd -m -s /usr/pkg/bin/bash -p `pwhash qwerty$i` user$i;
done
```
### PAM
#Error
Modificar /etc/pam.d/login para bloquear root por cli y /etc/pam.d/xdm para bloquer root por login grafico
```
auth required pam_succeed_if.so user != root quiet_success
```
# ATENCION
Lo de arriba de pam no va porque no encuentra pam_succeed_if.so, mi solucion fue poner en el .xsession de root exit 1, pero eso no lo bloquea de las terminales sin sesión gráfica jajaajaj super cutre pero trabajo honesto


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
	auth sufficient pam_group.so group=topWheel root_only
```

### Añadir fecha de caducidad
```
usermod -e "apr 08 2023" user100
usermod -e "apr 08 2023" user200
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