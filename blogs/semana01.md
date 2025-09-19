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

## 2.1 Configuración del entorno de desarrollo

`git clone --recurse-submodules https://github.com/JdeRobot/RoboticsAcademy.git -b <src-branch>`: Clona el repositorio principal de Robotics Academy en una rama específica.

`cd RoboticsAcademy/`: Accede al directorio RoboticsAcademy/.

`./scripts/develop_academy.sh -r <link to the RAM repo/fork> -b <branch of the RAM repo> -i <humble>`: Ejecuta el script de configuración (se puede ejecutar sin argumentos).

`http://127.0.0.1:7164/exercises/`: Dirección WEB de la interfaz de Robotics Academy.

`./scripts/develop_academy.sh -h`: Muestra información sobre las opciones disponibles para ejecutar el script.

**IMPORTANTE**: Tras ejecutar el script, se crea la carpeta src, que contiene todos los archivos de RoboticsApplicationManager. Desde esa carpeta, se pueden crear ramas y confirmar cambios en el repositorio de RAM. Para el resto de los cambios, también se puede trabajar normalmente desde la carpeta RoboticsAcademy, donde el contenido de la carpeta src, junto con los archivos repetitivos comunes, se ignora automáticamente. Y por último, cuando se quiera terminar de desarrollar, se cierra el script con Ctrl+C.

## 2.2 Problemas que pueden surgir

PROBLEMA 1: Instalación incorrecta de alguna dependencia o alguna ruta no agregada.

PROBLEMA 2: No se lanza el frontend.

### 2.2.1 Solución 1: Lanzar el frontend desde otra terminal

TERMINAL 1

`cd /RoboticsAcademy`

TERMINAL 2

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash`
`nvm install 20`
`nvm use 20`
`npm install --global yarn`
`cd react_frontend/ && yarn install && yarn run dev`

### 2.2.2 Solución 2: Generar un RADI (borrar la imagen generada y volver a hacerlo)

`cd /scripts/RADI`: Accede al directorio en el que se encuentra el script.

`chmod +x build.sh`: Concede permisos al equipo para crear imágenes Docker.

`./build.sh -h`: Muestra información adicional sobre el script build.sh.

`./build.sh -a [ROBOTICS_ACADEMY] -i [ROBOTICS_INFRASTRUCTURE] -m [RAM] -r [ROS_DISTRO] -t [IMAGE_TAG]`: Construye la imagen de Docker.

- [ROBOTICS_ACADEMY]: Este es el nombre de la rama del repositorio de Robotics Academy que se usará. El valor predeterminado es humble-devel.

- [ROBOTICS_INFRASTRUCTURE]: Este es el nombre de la rama del repositorio de Robotics Infrastructure que se utilizará. El valor predeterminado es humble-devel.

- [RAM]: Este es el nombre de la rama del repositorio RoboticsApplicationManager que se usará. El valor predeterminado es humble-devel.

- [ROS_DISTRO]: Esta es la distribución de ROS que se debe usar. El script actualmente es compatible con humble. El valor predeterminado es "humble".

- [IMAGE_TAG]: Esta es la etiqueta de la imagen de Docker que se creará. El valor predeterminado es test.

### 2.2.3 Solución 3: Buscar imágenes instaladas

`sudo docker images`: Muestra qué imágenes del Docker tengo instaladas.

`sudo docker rmi [image_id]`: Elimina imágenes del Docker ya instaladas.

`sudo docker ps -a`: Enumera todos los contenedores instalados.

`sudo docker rm [docker_id]`: Elimina todos los contenedores que utilizan la imagen que se desea eliminar.

### 2.3 Uso de Docker Run

`cd common`
`cd console_interfaces`
`zip -r ../common.zip console_interfaces/`
`cd ..`
`cd gui_interfaces`
`zip -r -u ../common.zip gui_interfaces/`
`cd ..`
`cd hal_interfaces`
`zip -r -u ../common.zip hal_interfaces/`
`cd ../..`
`mv common/common.zip react_frontend/src/common.zip`

docker run --hostname my-postgres --name academy_db -d\
    -e POSTGRES_DB=academy_db \
    -e POSTGRES_USER=user-dev \
    -e POSTGRES_PASSWORD=robotics-academy-dev \
    -e POSTGRES_PORT=5432 \
    -d -p 5432:5432 \
    jderobot/robotics-database:latest

`docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all" || echo "") --device /dev/dri -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Selección automática de GPU.

`docker run --rm -it --device /dev/dri -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Selección automática de GPU (sin NVIDIA).

`docker run --rm -it -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Sólo CPU.

## 2.4 Uso de NVIDIA

`sudo apt-get update`: Actualiza todos los paquetes.

`sudo apt-get install -y nvidia-docker2`: Instala el runtime de nvidia para usarlo como Docker.

`sudo systemctl restart docker`: Reinicia el servicio Docker para actualizar la nueva configuración.

`docker info | grep -i runtime`: Comprueba si Docker reconoce a NVIDIA como un nuevo entorno de ejecución (lo más probable es que no lo reconozca). Por ello, habrá que hacerlo manualmente editando y/o creando el fichero `/etc/docker/daemon.json` de la siguiente forma:

{
  "runtimes": {
    "nvidia": {
      "path": "nvidia-container-runtime",
      "runtimeArgs": []
    }
  }
}

`dpkg -l | grep nvidia-container-runtime`: Comprueba si nvidia-runtime está instalado.

`sudo apt-get install -y nvidia-container-runtime`: Instala nvidia-runtime.

`sudo systemctl restart docker`: Reinicia el servicio de Docker para actualizar la configuración y comprobar que todo funciona correctamente.

## 2.5 Agregar un nuevo ejercicio

### 2.5.1 Crear la carpeta de ejercicios con el código fuente y el frontend

`mkdir exercises/static/exercises/exercise_id`
`cd exercises/static/exercises/exercise_id`

`mkdir python_template/<ros_version>`
`cd python_template/<ros_version>`
`touch WebGUI.py`: Interacciones entre el ejercicio y el frontend.
`touch HAL.py`: Capa de abstracción de hardware para acceder a los datos del robot.
`touch Frequency.py`: Control de frecuencia del código iterativo.

`mkdir react-components`
`cd react-components`
`touch WebGUI.js`: Interfaz de ejercicios.

### 2.5.2 Añadir el ejercicio a la base de datos

**IMPORTANTE**: Es necesario añadir el frontend a la carpeta `exercises/templates/exercises` y crear dicha entrada en `database/exercise/db.sql`.

PASO 1: Inicie el contenedor de forma normal.

PASO 2: Acceda a `http://127.0.0.1:7164/admin/` en un navegador e inicie sesión con "usuario" y "contraseña".

PASO 3: Haga clic en "agregar ejercicio" y complete los campos obligatorios que se especifican a continuación.

PASO 4: Abra un shell en el contenedor universe_db: `docker exec -it universe_db bash`.

PASO 5: Volcar los cambios usando `./scripts/saveDb.sh`.

Una entrada de ejercicio en la base de datos debe incluir los siguientes datos:

`exercise id`: Identificador único de ejercicio, debe coincidir con el nombre de la carpeta.

`name`: Nombre para mostrar en la lista de ejercicios.

`description`: Descripción para mostrar en la lista de ejercicios.

`tags`: Un ejercicio debe incluir al menos una etiqueta ROS ("ROS2"). El ejercicio solo se mostrará en la lista de ejercicios cuando la versión de ROS de RoboticsBackend instalada aparezca en las etiquetas. La barra de búsqueda también utiliza etiquetas.

`status`: cambia el indicador de estado (ACTIVO = verde; PROTOTIPO = amarillo; INACTIVO = rojo).

`url`: URL de la documentación del ejercicio.

## 2.6 Agregar cambios locales al Robotics Backend con cambios bidireccionales

Este método es para saber si se ha trabajado hasta ahora en su configuración local y busca importar todos los cambios dentro de Robotics Backend y al mismo tiempo poder editar y conservar cambios adicionales.

PASO 1: En la Terminal, abra el directorio donde se encuentra el proyecto / código. Por ejemplo: `cd ~/my_project`.

PASO 2: Añade `-v $(pwd):/location_in_radi` a tu comando CLI `docker run` utilizado para ejecutar el contenedor. (Ejemplo: `docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all" || echo "") --device /dev/dri -p 7164:7164 -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 jderobot/robotics-backend -v $(pwd):/home jderobot/robotics-academy`).

Esto importará su directorio local dentro del contenedor de Docker, si ha utilizado el comando de ejemplo como el anterior, donde la ubicación donde se ejecuta el comando está montada en la carpeta de inicio dentro del contenedor de Docker, simplemente podrá ver todos los directorios locales montados dentro de /home de RoboticsBackend.

Para asegurarse de que su directorio local se haya montado correctamente en la ubicación correcta dentro de Robotics Backend, navegue a `http://localhost:1108/vnc.html` después de iniciar un ejercicio (esto implica hacer clic en el botón de inicio de cualquier ejercicio de su elección) y esto abrirá una instancia de consola vnc donde puede verificar la integridad del montaje.