# Administración de Pools de Almacenamiento en KVM

Este documento describe cómo gestionar pools de almacenamiento en KVM utilizando la herramienta `virsh`. Los pools de almacenamiento son una forma de organizar y administrar los recursos de almacenamiento que se utilizan para las máquinas virtuales.

## Listar Pools Existentes

Para listar todos los pools de almacenamiento disponibles, utiliza el siguiente comando:

```bash
virsh pool-list --all
```

Este comando mostrará todos los pools de almacenamiento configurados, indicando cuáles están activos y cuáles no.

## Crear un Pool de Almacenamiento

Para crear un nuevo pool de almacenamiento, utiliza el comando `virsh pool-define-as`. Aquí hay un ejemplo para crear un pool de tipo `dir`:

```bash
virsh pool-define-as --name nombre_del_pool --type dir --target /ruta/al/directorio
```

### Parámetros:
- `--name`: Nombre del pool de almacenamiento.
- `--type`: Tipo de almacenamiento (ej. `dir`, `fs`, `logical`, etc.).
- `--target`: Ruta donde se almacenarán los volúmenes del pool.

## Iniciar y Detener un Pool

Para iniciar un pool de almacenamiento (activarlo):

```bash
virsh pool-start nombre_del_pool
```

Para detener un pool de almacenamiento (desactivarlo):

```bash
virsh pool-destroy nombre_del_pool
```

### Configuración Automática
Para asegurarse de que el pool se inicie automáticamente cuando se reinicia el sistema, use:

```bash
virsh pool-autostart nombre_del_pool
```

## Eliminar un Pool de Almacenamiento

Para eliminar un pool de almacenamiento, primero debes detenerlo y luego eliminarlo:

1. **Detener el pool**:
   ```bash
   virsh pool-destroy nombre_del_pool
   ```

2. **Eliminar la definición del pool**:
   ```bash
   virsh pool-undefine nombre_del_pool
   ```

3. **Eliminar los datos del pool (opcional)**:
   Si deseas eliminar todos los datos asociados con el pool, puedes borrar manualmente el directorio del pool:
   ```bash
   rm -rf /ruta/al/directorio
   ```

⚠️ **Advertencia:** Asegúrate de que el directorio no contenga datos importantes antes de eliminarlo.

## Referencias

- [Documentación oficial de libvirt](https://libvirt.org/)
