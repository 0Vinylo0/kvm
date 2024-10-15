# Creación de Máquinas Virtuales en KVM (Kernel-based Virtual Machine)

En este documento, aprenderás a crear y configurar máquinas virtuales (MV) en KVM utilizando herramientas como `virt-manager` y la línea de comandos con `virt-install`. Esto te permitirá crear entornos virtuales para distintos sistemas operativos.

## Requisitos Previos

- KVM y sus componentes instalados. Si no lo has hecho, consulta la [guía de instalación de KVM](instalacion_kvm.md).
- Archivo de instalación ISO del sistema operativo que deseas instalar.

## Método 1: Crear una Máquina Virtual usando virt-manager (Interfaz Gráfica)

1. Abre `virt-manager` desde la terminal:
   ```bash
   virt-manager
   ```

2. Haz clic en **"Crear nueva máquina virtual"** en la ventana principal.

3. Selecciona la fuente de instalación:
   - Elige **"Instalar desde archivo de imagen (ISO)"** y selecciona el archivo ISO del sistema operativo.
   - Selecciona el tipo y versión del sistema operativo.

4. Asigna la memoria RAM y el número de CPUs a la máquina virtual según tus necesidades.

5. Configura el almacenamiento de la MV:
   - Elige el tamaño del disco virtual.
   - Puedes seleccionar un disco virtual existente o crear uno nuevo.

6. Configura la red de la máquina virtual:
   - Puedes usar la red NAT predeterminada o una configuración de red bridge.

7. Revisa las configuraciones y haz clic en **"Finalizar"** para crear y arrancar la máquina virtual.

## Método 2: Crear una Máquina Virtual usando virt-install (Línea de Comandos)

La herramienta `virt-install` permite crear máquinas virtuales mediante la línea de comandos, lo cual es útil para automatizar tareas.

### Crear una MV con virt-install

Usa el siguiente comando para crear una nueva máquina virtual con las configuraciones básicas:

```bash
sudo virt-install \
--name nombre_de_la_mv \
--vcpus 2 \
--memory 2048 \
--cdrom /ruta/al/archivo.iso \
--disk size=20 \
--os-type linux \
--os-variant ubuntu20.04 \
--network bridge=br0 \
--graphics spice
```

### Descripción de los parámetros:

- `--name`: Nombre de la máquina virtual.
- `--vcpus`: Número de CPUs asignadas a la MV.
- `--memory`: Cantidad de memoria RAM en MB.
- `--cdrom`: Ruta al archivo ISO del sistema operativo.
- `--disk size=20`: Tamaño del disco duro virtual en GB.
- `--os-type`: Tipo de sistema operativo (linux, windows, etc.).
- `--os-variant`: Variante específica del sistema operativo (ej., ubuntu20.04).
- `--network`: Configuración de red, puede ser bridge o NAT.
- `--graphics`: Configuración gráfica para la MV.

## Comandos Útiles para la Gestión de Máquinas Virtuales

- **Listar máquinas virtuales activas:**
  ```bash
  virsh list
  ```

- **Listar todas las máquinas virtuales (activas e inactivas):**
  ```bash
  virsh list --all
  ```

- **Iniciar una máquina virtual:**
  ```bash
  virsh start nombre_de_la_mv
  ```

- **Apagar una máquina virtual:**
  ```bash
  virsh shutdown nombre_de_la_mv
  ```

- **Eliminar una máquina virtual:**
  ```bash
  virsh undefine nombre_de_la_mv --remove-all-storage
  ```

## Solución de Problemas Comunes

- **Problema:** La máquina virtual no se conecta a la red.
  - **Solución:** Verifica que la red virtual esté configurada correctamente y que esté activa usando `virsh net-list --all`.

- **Problema:** No se puede iniciar la máquina virtual.
  - **Solución:** Asegúrate de que KVM esté instalado correctamente y que el servicio `libvirtd` esté activo:
    ```bash
    sudo systemctl status libvirtd
    ```

## Recursos adicionales

- [Documentación oficial de virt-install](https://linux.die.net/man/1/virt-install)
- [Guía de virt-manager](https://virt-manager.org/)
