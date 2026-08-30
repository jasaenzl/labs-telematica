# Instalación Ubuntu

## Prerrequisitos

1. Desactivar aislamiento de núcleo de Windows.

    - Abrir el menú Inicio y buscar Seguridad de Windows.
    - Seleccionar la opción Seguridad del dispositivo en el menú lateral o principal.
    - Buscar el apartado de Aislamiento del núcleo y hacer clic en Detalles de aislamiento del núcleo.
    - Localizar la opción Integridad de memoria.
    - Poner el interruptor en Desactivado.
    - Reiniciar el equipo para aplicar los cambios.

2. Desactivar la virtualización nativa en Windows 11.

   - Abrir las características de Windows.
     - Presionar la tecla Windows.
     - Escribir "Activar o desactivar las características de Windows".
     - Presionar Enter para abrir la ventana de configuración.
   - Desactivar los servicios.
     - Desmarcar Hyper-V (esto desactivará todos sus componentes).
     - Desmarcar Plataforma de máquina virtual.
     - Desmarcar Plataforma de hipervisor de Windows.
   - Aplicar y reiniciar.
     - Hacer click en el botón Aceptar.
     - Aplicar los cambios.
     - Hacer click en Reiniciar ahora para completar el proceso.

3. Descargar e instalar VMware.

    - Ingresar a <https://www.vmware.com/>.
    - Hacer click en Login → Broadcom Support.
    - Crear una cuenta.
    - Hacer click en Software → "My Downloads".
    - Hacer click en Free Software.
    - Descargar VMware Workstation Pro.

4. Descargar Ubuntu.

    - Ingresar a <https://ubuntu.com/>.
    - Hacer click en "Get Ubuntu" → Download Ubuntu Desktop.
    - Hacer click en Download (versión Intel or AMD 64-bit).

## Instalación

### Crear e instalar la máquina virtual de Ubuntu

Crear una nueva máquina virtual en VMware Workstation Pro utilizando la imagen ISO de Ubuntu descargada previamente.

**Recomendaciones mínimas de configuración:**

- **Procesador:** 1 procesador con 8 núcleos o más.
- **Memoria RAM:** 12 GB o más.
- **Disco duro:** 100 GB o más.
- **Adaptador de red:** configurar en modo Puente (Bridged). Si esta configuración no funciona correctamente, utilizar NAT.
- **Virtualización del procesador:** habilitar la opción Virtualize Intel VT-x/EPT or AMD-V/RVI.

Instalar Ubuntu utilizando estas recomendaciones.

### VM Tools

VM Tools permite mejorar la integración entre Ubuntu y VMware, incluyendo el manejo de la resolución de pantalla, el uso del portapapeles y otras funciones de integración entre el sistema anfitrión y la máquina virtual.

```bash
sudo apt update
sudo apt install open-vm-tools open-vm-tools-desktop -y
sudo reboot
```

### GNS3

Estas instrucciones son para Ubuntu y todas las distribuciones basadas en él (como Linux Mint).

``` bash
sudo add-apt-repository ppa:gns3/ppa
sudo apt update
sudo apt install gns3-gui gns3-server
```

Cuando se pregunte si los usuarios que no son root deben tener permiso para utilizar Wireshark y ubridge, seleccionar «Sí» en ambas ocasiones.

Si se desea compatibilidad con IOU:

```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install gns3-iou
```

Para instalar Docker-CE:

Eliminar versiones antiguas:

```bash
sudo apt remove docker docker-engine docker.io
sudo snap remove docker
```

Instalar los siguientes paquetes:

``` bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

Crear el directorio para las llaves (si no existe):

``` bash
sudo mkdir -p /etc/apt/keyrings
```

Descargar la llave de Docker:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Dar permisos:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Agregar el repositorio:

``` bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Instalar Docker-CE:

```bash
sudo apt update
sudo apt install docker-ce
```

Finalmente, agregar su usuario a los siguientes grupos:

ubridge libvirt kvm wireshark docker

```bash
sudo usermod -aG ubridge,libvirt,kvm,wireshark,docker $(whoami)
```

Cerrar sesión y volver a iniciarla para aplicar los cambios.

### TigerVNC Viewer

Si se requiere utilizar VNC para acceder a alguna máquina virtual, instalar TigerVNC Viewer:

```bash
sudo apt update
sudo apt install tigervnc-viewer -y
```
