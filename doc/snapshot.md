# Snapshots en KVM

## ¿Qué es una Snapshot en KVM?

En el contexto de **KVM (Kernel-based Virtual Machine)**, una **snapshot** es una captura del estado actual de una máquina virtual (VM) en un momento específico. Esta captura incluye la memoria, el estado de la CPU, el disco y los dispositivos virtuales. Las snapshots son útiles para realizar copias de seguridad rápidas, probar actualizaciones o cambios sin riesgo y restaurar el estado de la VM si algo sale mal.

## Beneficios de usar Snapshots en KVM

- **Restauración rápida**: Permite volver a un estado anterior de la VM si ocurre algún problema.
- **Pruebas seguras**: Ideal para probar cambios en el sistema operativo o aplicaciones sin afectar la configuración estable.
- **Copia de seguridad**: Una manera efectiva de tener un respaldo completo del estado actual de la máquina virtual.

## Tipos de Snapshots en KVM

- **Snapshot de sistema completo (completo)**: Captura el estado de la VM incluyendo la memoria, CPU y el disco.
- **Snapshot de disco (disco)**: Solo captura el estado del disco sin incluir la memoria y otros estados del sistema.

## Cómo Crear Snapshots en KVM

Para crear snapshots en KVM, se recomienda utilizar la herramienta `virsh`, que es una interfaz de línea de comandos para gestionar máquinas virtuales. A continuación se presentan los comandos más comunes para trabajar con snapshots:

## Crear una Snapshot Completa

```bash
virsh snapshot-create-as <nombre-de-la-vm> <nombre-de-la-snapshot> \
--description "Descripción de la snapshot" \
--disk-only --atomic \
```

- `nombre-de-la-vm`: Nombre de la máquina virtual.
- `nombre-de-la-snapshot`: Nombre que quieres darle a la snapshot.
- `--disk-only`: Indica que solo se debe capturar el disco, sin la memoria.
- `--atomic`: Garantiza que la snapshot se cree de forma segura.

## Listar Snapshots Existentes

Para ver las snapshots que has creado para una VM específica:

```bash
virsh snapshot-list <nombre-de-la-vm>
```

## Restaurar una Snapshot
Para restaurar una snapshot a su estado original:

```bash
virsh snapshot-revert <nombre-de-la-vm> <nombre-de-la-snapshot>
```

Esto restaurará la VM al estado exacto en el que estaba cuando se creó la snapshot.

## Eliminar una Snapshot
Para eliminar una snapshot que ya no necesitas:

```bash
virsh snapshot-delete <nombre-de-la-vm> <nombre-de-la-snapshot>
```

## Referencias

- [Documentacion oficial libvirt](https://libvirt.org/)
