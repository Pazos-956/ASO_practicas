# Solaris
## Crear zona 
Primero crear el file system donde va a residir la zona:
`zfs create rpool/<zona>`
Para crear la zona y configuración:(el ultimo comando es para instalarla)
```
zonecfg -z zonita
create
set zonepath=/rpool/zonita
set autoboot=true (para iniciar en boot)
set bootargs="-m verbose"
verify
commit
exit
zoneadm -z zonita install
```
Para listar zonas: `zoneadm list -icv`
Para bootear la zona: `zoneadm -z zonita boot`
Para loguear en la consola de la zona: 
`zlogin -C zonita` (La primera vez sale la configuración como si fuera una instalación del so)
para salir `~.`(solaris no me deja escribir eso y no puedo salir, crear terminal nueva xd)

