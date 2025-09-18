# 1. Robotics Academy (RA): Guía de Usuario

## 1.1 Instalación de Docker

### 1.1.1 Compatibilidad con la virtualización KVM

`modprobe kvm`: Carga manualmente el módulo kvm para verificar si el host es compatible con la virtualización (kvm_intel o kvm_amd).

`kvm-ok`: Muestra los diagnósticos si los comandos anteriores fallan.

`lsmod | grep kvm`: Comprueba si los módulos KVM están habilitados.

`ls -al /dev/kvm`: Comprueba la propiedad de /dev/kvm.

`sudo usermod -aG kvm $USER`: Agrega al usuario al grupo kvm para poder acceder al dispositivo.

### 1.1.2 Instalación de Docker Desktop en Ubuntu 24.04

`sudo apt install gnome-terminal`: Habilita el acceso a la terminal desde Docker Desktop.

`sudo apt-get update`: Actualiza todos los paquetes.

`sudo apt-get install ./docker-desktop-amd64.deb`: Instala Docker a través del paquete DEB.

**IMPORTANTE**: Ignorar el error que aparece al final del proceso de instalación.

`N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)`

### 1.1.3 Inicialización de Docker Desktop en Ubuntu 24.04

PASO 1: Navegue hasta la aplicación Docker Desktop en su escritorio Gnome/KDE.

PASO 2: Seleccione Docker Desktop para iniciar Docker. Se muestra el Acuerdo de servicio de suscripción de Docker.

PASO 3: Seleccione Aceptar para continuar. Docker Desktop se iniciará después de aceptar los términos.

**IMPORTANTE**: Tenga en cuenta que Docker Desktop no se ejecutará si no acepta los términos. Puede aceptarlos más adelante abriendo Docker Desktop.

`systemctl --user start docker-desktop`: Ejecutar el siguiente comando en otra terminal.

### 1.1.4 Verificación de las versiones de los binarios

`docker compose version`: Primera alternativa.

`docker --version`: Segunda alternativa.

`docker version`: Tercera alternativa.

### 1.1.5 Cambiar estado del docker

Para permitir que Docker Desktop se inicie al iniciar sesión, desde el menú de Docker, seleccione Configuración > General > Iniciar Docker Desktop cuando inicie sesión en su computadora.

`systemctl --user enable docker-desktop`: Comando alternativo a lo anterior.

Para detener Docker Desktop, seleccione el ícono del menú de Docker para abrir el menú de Docker y seleccione Salir de Docker Desktop.

`systemctl --user stop docker-desktop`: Comando alternativo a lo anterior.

`sudo apt-get install ./docker-desktop-amd64.deb`: Actualizar Docker Destop.

## 1.2 Uso del Docker de Robotics Academy

`docker pull jderobot/robotics-database:latest`: Extrae la base de datos de Robotics Academy.

`docker pull jderobot/robotics-academy:latest`: Extrae la distribución actual de Robotics Academy.

## 1.3 Lanzamiento del Docker de Robotics Academy

docker run --hostname my-postgres --name academy_db -d\
    -e POSTGRES_DB=academy_db \
    -e POSTGRES_USER=user-dev \
    -e POSTGRES_PASSWORD=robotics-academy-dev \
    -e POSTGRES_PORT=5432 \
    -d -p 5432:5432 \
    jderobot/robotics-database:latest

`docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all" || echo "") --device /dev/dri -p 6080:6080 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: NVIDIA > Intel / AMD > Solo CPU.

`docker run --rm -it --device /dev/dri -p 6080:6080 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: GPU integrada.

`docker run --rm -it -p 6080:6080 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Sólo CPU.

`docker ps -a`: Localiza el contenedor Docker utilizado para Robotics Academy.

`docker rm CONTAINER_ID`: Retira el contenedor de Robotics Academy a partir de su ID.

`127.0.0.1:7164/`: IP de Unibotics en la máquina local.

## 1.4 Instalación del Robotics Backend

`docker pull jderobot/robotics-backend:latest`: Extrae la distribución actual del Robotics Backend (versión 4.8.0).

## 1.5 Inicialización del Robotics Backend

`docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all" || echo "") --device /dev/dri -p 6080:6080 -p 1108:1108 -p 7163:7163 jderobot/robotics-backend:latest`: NVIDIA > Intel / AMD > Sólo CPU.

`docker run --rm -it --device /dev/dri -p 6080:6080 -p 1108:1108 -p 7163:7163 jderobot/robotics-backend:latest`: GPU integrada.

`docker run --rm -it -p 6080:6080 -p 1108:1108 -p 7163:7163 jderobot/robotics-backend:latest`: Sólo CPU.

`docker ps -a`: Localiza el contenedor Docker utilizado para Robotics Backend.

`docker rm CONTAINER_ID`: Retira el contenedor de Robotics Backend a partir de su ID.

# 2. Robotics Academy (RA): Instrucciones para Desarrolladores