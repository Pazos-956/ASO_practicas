# FEDORA:w

```
dnf install whois (para mkpasswd)
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
	auth sufficient pam_wheel.so group=topWheel root_only trust use_uid
	auth required pam_wheel.so use_uid
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

# FREEBSD

Script para crear usuarios con contra:
```
#!/bin/bash

for i in `seq -w 0 999`; do
	echo "qwerty$i" | pw useradd user$i -m -h 0;
done
```
### PAM
Modificar /etc/pam.d/login para bloquear root por cli y /etc/pam.d/lightdm para bloquer root por login grafico !!!!!!!!NO VA, CUIDADO¡¡¡¡¡¡¡¡¡
```
account required pam_group.so group=root deny luser
```
Lo de arriba de pam no va no se por que, mi solucion fue poner en el .xsession de root exit 1, como en netbsd, pero aqui no soy capaz de ir al login sin interfaz grafica asi que todo joya supongo

Para que solo user444 user555 y user666 hagan su sin contraseña y usuario lo haga con contra:
```
groupadd wheel
groupadd topWheel
pw usermod -g wheel -n user444
pw usermod -g wheel -n user555
pw usermod -g wheel -n user666
pw usermod -g wheel -n usuario
pw usermod -g topWheel -n user444
pw usermod -g topWheel -n user555
pw usermod -g topWheel -n user666
vim /etc/pam.d/su
	auth sufficient pam_group.so no_warn group=topwheel root_only
```

### Añadir fecha de caducidad
```
pw usermod -e 08-apr-2023 -n user100
pw usermod -e 08-apr-2023 -n user200
```
### Hacerte pasar por user013
```
vim /etc/pam.d/su
	auth sufficient pam_group.so no_warm group=user013 luser=user013
```
