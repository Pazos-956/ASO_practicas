# Ubuntu Server
```
apt install lxc
apt install lxc-templates
```
## Crear Container lxc e iniciarlo
```
lxc-create -t ubuntu -n containter
lxc-start -n container
vim /var/lib/lxc/container/config
	lxc.start.auto = 1
lxc-attach container
useradd -m elbueno
useradd -m elmalo
useradd -m elfeo
```
Para listar los container y ver su info: `lxc-ls -f`
la carpeta donde se almacena es `/var/lib/lxc/container/rootfs/`
Hice la configuración de la diapo 158 para correr lxc como usuario normal, pero por la cara porque creo que no es necesario.