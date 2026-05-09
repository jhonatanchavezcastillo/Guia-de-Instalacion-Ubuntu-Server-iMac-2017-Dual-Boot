# Guia-de-Instalacion-Ubuntu-Server-iMac-2017-Dual-Boot
Guía de Instalación: Ubuntu Server en iMac 2017 (Dual Boot)
1. Preparación del Disco en macOS
Modo Recuperación: Arrancar con Command (⌘) + R.

Limpieza de Volúmenes: En Utilidad de Discos, seleccionar "Mostrar todos los dispositivos". Borrar el disco físico raíz como APFS con esquema Mapa de particiones GUID.

Particionado: Crear una partición secundaria (ej. 100 GB) en formato MS-DOS (FAT). Esta será el contenedor para Ubuntu.

Instalación base: Reinstalar macOS Catalina en la partición principal de 600 GB.

2. Creación del USB Booteable (Desde Windows)
Herramienta: Rufus (Versión Portable).

ISO: Ubuntu Server (64-bit PC AMD64).

Configuración Crítica:

Esquema de partición: GPT.

Sistema de destino: UEFI (no CSM).

Sistema de archivos: FAT32.

Modo de escritura: Imagen ISO (Recomendado).

3. Proceso de Instalación de Ubuntu
Arranque: Conectar USB y encender presionando la tecla Option (⌥). Seleccionar EFI Boot.

Idioma y Teclado:

Sistema: English (para facilitar debug de logs).

Teclado: Spanish (para usar Option + 3 para el símbolo #).

Almacenamiento: Seleccionar Custom storage layout.

Formatear la partición FAT previa como ext4.

Punto de montaje: /.

Software: Marcar obligatoriamente Install OpenSSH server.

4. Configuración de Red (Netplan WiFi)
Para conectar el servidor sin interfaz gráfica a la red local, se debe editar el archivo YAML de configuración:

Comando: sudo nano /etc/netplan/00-installer-config.yaml

Configuración (Respetar espacios, no usar Tabs):

YAML
network:
  version: 2
  renderer: networkd
  wifis:
    wlp2s0:
      dhcp4: true
      access-points:
        "Totalplay-689E":
          password: "tu_password"
Aplicación:

sudo netplan generate

sudo netplan apply

5. Acceso Remoto y Desarrollo
Desde Windows (Terminal): ssh usuario@ip_del_servidor

VS Code: Instalar extensión Remote - SSH para programar directamente en el sistema de archivos de Ubuntu.

Entorno: Con 8 GB de RAM y Ubuntu Server (consumo base <500 MB), el sistema queda optimizado para levantar contenedores Docker, instancias de Redis y workers de BullMQ.
