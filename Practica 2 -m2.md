# Ubuntu
Tras instalar Ubuntu server:
crear carpeta en /boot/efi/EFI/
mover initrd y vmlinuz ahi
Modificar /etc/grub.d/40_custom para añadir las entradas (para ver UUID usar blkid) (se pone el UUID de la particion /)
Hice estos tres porque no iba
``grub-install
``update-grub
``grub-mkconfig -o /boot/grub/grub.cfg
### Los pasos correctos son:
Tras modificar 40_custom entrar en /etc/default/grub para ver la configuración del  grub, si timeout está a 0 cambiar a otro número, y si GUB_TIMEOUT_STYLE está en 'countodwn' o 'hidden' tampoco se mostrará el grub, hay que cambiarlo a 'menu',
tras eso guardar y ejecutar un ``update-grub

## rEFInd
Para instalar refind se puede usar ``apt install refind``, aceptar lo que te sale y configurar archivo /boot/efi/EFI/refind/refind.conf
buscar scanfor y dejar solo manual (para que solo salga lo que esté en la config)
añadir las entradas al final del todo y despues de guardar rebootear (se aplica  automáticamente)