# Gestión de Discos en KVM

Este documento proporciona una guía sobre cómo gestionar discos en máquinas virtuales (VM) utilizando la herramienta `virsh` en KVM. Se incluirán operaciones comunes como agregar, listar, modificar y eliminar discos en las VM, así como una descripción de los formatos de disco más utilizados.

## Formatos de Disco

KVM admite varios formatos de disco, pero los dos más comunes son:

1. **QCOW2 (QEMU Copy-On-Write Version 2)**:
   - Soporta características avanzadas como snapshots (instantáneas), compresión y cifrado.
   - Permite un uso eficiente del espacio, ya que el archivo del disco crece dinámicamente según las necesidades.
   - Ideal para entornos donde se requiere flexibilidad y gestión avanzada del almacenamiento.

2. **RAW**:
   - Es el formato más simple y directo, lo que significa que ofrece un rendimiento más alto.
   - El archivo ocupa todo el espacio asignado en disco desde el principio, lo que puede ser menos eficiente en términos de almacenamiento.
   - Se recomienda cuando el rendimiento es una prioridad y no se necesitan características avanzadas como snapshots.

## Crear un Disco en Formato QCOW2
```bash
virsh vol-create-as --pool default --format qcow2 new-volumen.qcow2 1G
```

## Crear un Disco en Formato RAW
```bash
virsh vol-create-as --pool default --format raw new-volumen.qcow2 1G
```

### Parámetros:
- `--pool default`: Pool donde se alojara el nuevo volumen
- `--format qcow2` o `--format raw`: Define el formato del disco a crear.
- `new-volumen.qcow2`: Nombre del nuevo volumen.
- `1G`: Tamaño del disco que se desea crear en GB.

## Listar volumenes que hay en un pool

```bash
virsh vol-list default --details
```

## Eliminar volumen de un pool

```bash
virsh vol-delete --pool default disco2.qcow2
```

## Listar Discos en una VM

Para listar todos los discos adjuntos a una máquina virtual, utiliza el siguiente comando:

```bash
virsh domblklist nombre_de_la_vm
```

Este comando mostrará todos los discos y sus respectivos dispositivos asociados con la VM.

## Agregar un Disco a una VM

Para agregar un nuevo disco a una VM, utiliza el comando `virsh attach-disk`. A continuación se muestra un ejemplo para agregar un disco a una VM:

```bash
virsh attach-disk nombre_de_la_vm /ruta/al/disco.qcow2 vdb --cache none --persistent
```

### Parámetros:
- `nombre_de_la_vm`: Nombre o ID de la máquina virtual.
- `/ruta/al/disco.qcow2`: Ruta al archivo de imagen del disco (puede ser `.qcow2` o `.raw`).
- `vdb`: Nombre del dispositivo en el que se montará el disco dentro de la VM.
- `--cache none`: Configuración de caché para el disco.
- `--persistent`: Hace que el cambio sea permanente y persista después de un reinicio.

## Modificar un Disco

Para cambiar la configuración de un disco ya adjunto, primero se debe editar la definición del disco en el XML de la VM:

```bash
virsh edit nombre_de_la_vm
```

Busca la sección del disco que deseas modificar y cambia los parámetros según sea necesario. Una vez guardados los cambios, la configuración del disco se actualizará en la VM.

## Eliminar un Disco de una VM

Para desconectar y eliminar un disco de una máquina virtual, utiliza el siguiente comando:

```bash
virsh detach-disk nombre_de_la_vm vdb --persistent
```

### Parámetros:
- `nombre_de_la_vm`: Nombre o ID de la máquina virtual.
- `vdb`: Nombre del dispositivo del disco que se va a eliminar.
- `--persistent`: Hace que el cambio sea permanente.

## Referencias

- [Documentación oficial de libvirt](https://libvirt.org/)
