# Configuración de Redes en KVM (Kernel-based Virtual Machine)

En este documento, se detallará cómo configurar redes virtuales en KVM utilizando diferentes tipos de configuraciones como NAT, bridge (puente) y redes privadas. Estas configuraciones son esenciales para gestionar la conectividad de las máquinas virtuales.

## Tipos de Redes en KVM

1. **NAT (Network Address Translation):** Permite que las máquinas virtuales tengan acceso a la red externa mediante la traducción de direcciones. Es la configuración predeterminada en KVM.
2. **Bridge (Puente):** Permite que las máquinas virtuales se conecten directamente a la red local, como si fueran otros dispositivos físicos en la red.
3. **Redes Privadas:** Aisladas del acceso externo, estas redes solo permiten la comunicación entre máquinas virtuales en el mismo host.

## Paso 1: Verificar la configuración de red actual

Antes de realizar cambios, verifica las redes configuradas en tu entorno KVM:

```bash
virsh net-list --all
```

Esto mostrará las redes disponibles y su estado (activo o inactivo).

## Paso 2: Configurar una Red NAT

La configuración NAT es la más sencilla y permite que las máquinas virtuales tengan acceso a Internet mediante la red del host.

### Crear una Red NAT

1. Crea un archivo XML llamado `nat-network.xml` con el siguiente contenido:
   ```xml
   <network>
     <name>default-nat</name>
     <bridge name='virbr0' stp='on' delay='0'/>
     <forward mode='nat'/>
     <ip address='192.168.122.1' netmask='255.255.255.0'>
       <dhcp>
         <range start='192.168.122.2' end='192.168.122.254'/>
       </dhcp>
     </ip>
   </network>
   ```

2. Define y activa la red usando los comandos:
   ```bash
   sudo virsh net-define nat-network.xml
   sudo virsh net-start default-nat
   sudo virsh net-autostart default-nat
   ```

## Paso 3: Configurar una Red Bridge (Puente)

La configuración bridge permite que las máquinas virtuales tengan acceso directo a la red física del host.

### Crear una Red Bridge

1. Edita el archivo de configuración de red en el host (`/etc/network/interfaces` o equivalente según tu distribución) para crear un puente de red:
   ```bash
   auto br0
   iface br0 inet dhcp
       bridge_ports eno1
       bridge_stp off
       bridge_fd 0
       bridge_maxwait 0
   ```

   Reemplaza `eno1` con el nombre de tu interfaz de red física.

2. Reinicia el servicio de red para aplicar los cambios:
   ```bash
   sudo systemctl restart networking
   ```

3. Configura KVM para usar esta red bridge:
   ```xml
   <network>
     <name>bridge-network</name>
     <forward mode='bridge'/>
     <bridge name='br0'/>
   </network>
   ```

4. Define y activa la red:
   ```bash
   sudo virsh net-define bridge-network.xml
   sudo virsh net-start bridge-network
   sudo virsh net-autostart bridge-network
   ```

## Paso 4: Crear una Red Privada

Una red privada permite que las máquinas virtuales se comuniquen entre sí, pero no tienen acceso a la red externa ni al host.

### Configuración de Red Privada

1. Crea un archivo XML llamado `private-network.xml`:
   ```xml
   <network>
     <name>private-network</name>
     <bridge name='virbr1' stp='on' delay='0'/>
     <ip address='192.168.200.1' netmask='255.255.255.0'>
       <dhcp>
         <range start='192.168.200.2' end='192.168.200.254'/>
       </dhcp>
     </ip>
   </network>
   ```

2. Define y activa la red privada:
   ```bash
   sudo virsh net-define private-network.xml
   sudo virsh net-start private-network
   sudo virsh net-autostart private-network
   ```

## Paso 5: Verificar las Configuraciones de Red

Después de configurar las redes, verifica su estado con:
```bash
virsh net-list --all
```

Asegúrate de que las redes estén activas y configuradas correctamente.

## Deshabilitar una Red Virtual

Si necesitas deshabilitar alguna red en KVM, utiliza:
```bash
sudo virsh net-destroy nombre-de-la-red
sudo virsh net-undefine nombre-de-la-red
```

Reemplaza `nombre-de-la-red` con el nombre de la red que deseas desactivar.

## Recursos adicionales

- [Documentación oficial de redes en KVM](https://wiki.libvirt.org/page/Networking)
- [Guía de configuración de bridges en Linux](https://wiki.debian.org/BridgeNetworkConnections)
