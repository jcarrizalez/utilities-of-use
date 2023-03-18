Compartir sesiones SSH en Linux

Esto sirve para ver código en conjunto, dar soporte, colaborar en la instalación de algo entre devel y administradores, etc.

Para invitar a un usuario a que comparta la sesion ssh en linux (puede ser read-only, no se pongan nerviosos):

El primer usuario crea la sesion:

```bash
tmux -S /var/tmp/sharedtmux 
```

... y le da permiso al invitado para usar el mismo socket:

```
sudo chgrp [usuarioinvitado] /var/tmp/sharedtmu
```

El otro usuario entra por ssh y hace:

```
tmux -S /var/tmp/sharedtmux attach -r
```

Listo. Ambos ven lo mismo en la pantalla. Y pueden ser muchos los usuarios conectados.

Resumen técnico: Se usa "tmux" y se comparte la pantalla usando un socket file en /tmp al que tienen acceso ambos usuarios.

En vez de chgrp se puede hacer un setfacl, o crear un grupo de invitados, etc.

En vez de dar acceso ssh común se puede crear un usuario "invitado", ponerle como shell un script que corra "tmux -S /var/tmp/sharedtmux attach -r", y manejar quien puede entrar poniendo las claves publicas ssh de los usuarios en el .ssh/authorized_keys del usuario genérico (parecido a los que hace gitlab)
