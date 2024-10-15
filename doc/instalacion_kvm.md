# Instalación de KVM (Kernel-based Virtual Machine) en Linux

KVM (Kernel-based Virtual Machine) es una solución de virtualización completa para Linux que utiliza las extensiones de hardware de los procesadores para proporcionar virtualización. En este tutorial, aprenderás cómo instalar KVM en una distribución de Linux basada en Debian, como Ubuntu.

## Requisitos Previos

- Un procesador compatible con virtualización (Intel VT o AMD-V).
- Un sistema operativo basado en Debian (Ubuntu 20.04 o superior recomendado).
- Acceso de superusuario (root) o permisos de `sudo`.

## Paso 1: Verificar la compatibilidad de virtualización del hardware

Para verificar si tu CPU soporta virtualización, ejecuta el siguiente comando en la terminal:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

- Si el valor de salida es mayor que `0`, tu CPU soporta virtualización.
- Si el resultado es `0`, tu CPU no soporta virtualización y no podrás usar KVM.

## Paso 2: Actualizar el sistema

Antes de comenzar la instalación, es una buena práctica actualizar el sistema operativo. Ejecuta:

```bash
sudo apt update && sudo apt upgrade -y
```

## Paso 3: Instalar KVM y las herramientas necesarias

Para instalar KVM y los paquetes relacionados, usa el siguiente comando:

```bash
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

### Componentes instalados

- `qemu-kvm`: El paquete principal de KVM.
- `libvirt-daemon-system`: Daemon para gestionar máquinas virtuales.
- `libvirt-clients`: Herramientas de línea de comandos para la gestión de máquinas virtuales.
- `bridge-utils`: Herramientas para configurar redes bridge.
- `virt-manager`: Interfaz gráfica para gestionar máquinas virtuales.

## Paso 4: Verificar la instalación de KVM

Después de la instalación, verifica que el servicio de libvirt esté activo y ejecutándose:

```bash
sudo systemctl status libvirtd
```

La salida debería indicar que el servicio `libvirtd` está activo y funcionando correctamente.

## Paso 5: Añadir tu usuario al grupo `libvirt`

Para poder gestionar máquinas virtuales sin privilegios de superusuario, añade tu usuario al grupo `libvirt` y al grupo `kvm`:

```bash
sudo usermod -aG libvirt $(whoami)
sudo usermod -aG kvm $(whoami)
```

Luego, es necesario cerrar sesión y volver a iniciar sesión para que los cambios surtan efecto.

## Paso 6: Crear tu primera máquina virtual

Para crear una máquina virtual, puedes utilizar la interfaz gráfica `virt-manager`. Ejecuta el siguiente comando para abrirlo:

```bash
virt-manager
```

1. Haz clic en el botón **"Crear nueva máquina virtual"**.
2. Sigue las instrucciones para configurar el sistema operativo, la memoria, el almacenamiento y otros ajustes.
3. Inicia la máquina virtual una vez configurada.

## Paso 7: Verificación final

Para verificar que KVM está funcionando correctamente, ejecuta el siguiente comando:

```bash
virsh list --all
```

Deberías ver una lista de máquinas virtuales, aunque estén apagadas. Si no hay ninguna, significa que el entorno de KVM está listo para que crees nuevas máquinas virtuales.

## Desinstalación de KVM

Si necesitas desinstalar KVM por alguna razón, utiliza el siguiente comando:

```bash
sudo apt remove --purge qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y
sudo apt autoremove -y
```

## Recursos adicionales

- [Documentación oficial de KVM](https://www.linux-kvm.org/page/Main_Page)
- [Guía de virt-manager](https://virt-manager.org/)

