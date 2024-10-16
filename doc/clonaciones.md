# Clonación de Máquinas Virtuales en KVM

Este documento proporciona una guía detallada sobre cómo clonar máquinas virtuales (VM) utilizando herramientas como `virt-clone` y `virt-sysprep` en KVM. La clonación es útil para crear copias idénticas de una VM existente, lo cual es ideal para pruebas, desarrollo o despliegue rápido de entornos similares.

## Clonación de una Máquina Virtual

La clonación de una máquina virtual copia la configuración XML de la máquina de origen y sus imágenes de disco, y realiza ajustes en las configuraciones para asegurar la unicidad de la nueva máquina.

## Uso de virt-clone
Para clonar una máquina virtual existente, utiliza el siguiente comando básico:

```bash
virt-clone --original machine-name --name cloned-machine-name --file /var/lib/libvirt/images/new-volume-name.qcow2
```

### Opciones adicionales

`--original`: Nombre de la máquina virtual original.

`--name`: Nombre que deseas asignar a la máquina clonada.

`--file`: Ruta donde se guardará el nuevo disco clonado.

## Clonación automática
Si prefieres que el proceso sea más automatizado:

```bash
virt-clone --original machine-name --auto-clone
````
Esto generará automáticamente un nombre y un archivo de disco para el clon.

## Personalización del Clon
Después de clonar la máquina virtual, es importante modificar ciertos elementos para evitar conflictos con la VM original.

Uso de `virt-sysprep` se utiliza para "generalizar" la máquina clonada, eliminando configuraciones específicas como el ID de la máquina, las direcciones MAC y las claves SSH, para que el clon sea único:

```bash
virt-sysprep -d cloned-machine-name
```
Esto asegura que la nueva máquina clonada no tenga conflictos de red ni identidades duplicadas.

### Otras personalizaciones con virt-customize
Puedes usar virt-customize para realizar otras personalizaciones, como cambiar el hostname, inyectar claves SSH, o instalar paquetes adicionales:

```bash
virt-customize -d cloned-machine-name --hostname new-hostname --password user:password:NewPassword
```
## Problemas Comunes y Soluciones

Identidades duplicadas: Si no se utiliza `virt-sysprep`, la nueva máquina clonada puede tener la misma dirección MAC y claves SSH que la original.
Cambiar el nombre de la máquina: Utiliza virsh domrename para cambiar el nombre de la máquina clonada si es necesario:

```bash
virsh domrename old-machine-name new-machine-name
```
Problemas con el disco clonado: Asegúrate de que el archivo del disco clonado esté en la ubicación correcta y que tenga permisos adecuados.

## Referencias

- [Documentación oficial de libvirt](https://libvirt.org/)
- [virt-clone Manual](https://linux.die.net/man/1/virt-clone)
