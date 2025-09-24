# 1. Robotics Academy (RA): Guía de Usuario

## 1.1 Instalación de Docker

### 1.1.1 Compatibilidad con la virtualización KVM

`modprobe kvm`: Carga manualmente el módulo kvm para verificar si el host es compatible con la virtualización (`modprobe kvm_intel` o `modprobe kvm_amd`).

`kvm-ok`: Muestra los diagnósticos si los comandos anteriores fallan.

`lsmod | grep kvm`: Comprueba si los módulos KVM están habilitados.

`ls -al /dev/kvm`: Comprueba la propiedad de `/dev/kvm`.

`sudo usermod -aG kvm $USER`: Agrega al usuario al grupo kvm para poder acceder al dispositivo.

### 1.1.2 Instalación de Docker Desktop en Ubuntu 24.04

`sudo apt install gnome-terminal`: Habilita el acceso a la terminal desde Docker Desktop.

`sudo apt-get update`: Actualiza todos los paquetes.

`sudo apt-get install ./docker-desktop-amd64.deb`: Instala Docker a través del paquete DEB.

**IMPORTANTE**: Ignorar el error que aparece al final del proceso de instalación.

`N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)`

### 1.1.3 Inicialización de Docker Desktop en Ubuntu 24.04

**PASO 1**: Navega hasta la aplicación Docker Desktop ubicada en el escritorio Gnome / KDE.

**PASO 2**: Selecciona Docker Desktop para iniciar Docker, donde se muestra el acuerdo de servicio de suscripción de Docker.

**PASO 3**: Selecciona "Aceptar" para continuar. Después, Docker Desktop se iniciará después de aceptar los términos.

**IMPORTANTE**: Docker Desktop no se ejecutará si no se aceptan los términos (se pueden aceptar más adelante abriendo la aplicación Docker Desktop).

`systemctl --user start docker-desktop`: Ejecutar el siguiente comando en otra terminal.

### 1.1.4 Verificación de las versiones de los binarios

`docker compose version`: Primera alternativa para verificar las versiones de los binarios.

`docker --version`: Segunda alternativa para verificar las versiones de los binarios.

`docker version`: Tercera alternativa para verificar las versiones de los binarios.

### 1.1.5 Cambiar el estado del docker

Para permitir que Docker Desktop se inicie al iniciar sesión, se debe seleccionar desde el menú de Docker `Configuración > General > Iniciar Docker Desktop` cuando se inicie sesión.

`systemctl --user enable docker-desktop`: Comando alternativo a lo anterior.

Para detener Docker Desktop, se debe seleccionar el ícono del menú de Docker para abrir el menú de Docker y la opción `Salir de Docker Desktop`.

`systemctl --user stop docker-desktop`: Comando alternativo a lo anterior.

`sudo apt-get install ./docker-desktop-amd64.deb`: Actualiza Docker Destop.

## 1.2 Uso del Docker de Robotics Academy

`docker pull jderobot/robotics-database:latest`: Extrae la base de datos de Robotics Academy.

`docker pull jderobot/robotics-academy:latest`: Extrae la distribución actual de Robotics Academy.

## 1.3 Lanzamiento del Docker de Robotics Academy

```sh
docker run --hostname my-postgres --name academy_db -d\
    -e POSTGRES_DB=academy_db \
    -e POSTGRES_USER=user-dev \
    -e POSTGRES_PASSWORD=robotics-academy-dev \
    -e POSTGRES_PORT=5432 \
    -d -p 5432:5432 \
    jderobot/robotics-database:latest
```

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

`cd RoboticsAcademy/`: Accede al directorio `RoboticsAcademy/`.

`./scripts/develop_academy.sh -r <link to the RAM repo/fork> -b <branch of the RAM repo> -i <humble>`: Ejecuta el script de configuración (se puede ejecutar sin argumentos).

`http://127.0.0.1:7164/exercises/`: Dirección web de la interfaz de Robotics Academy.

`./scripts/develop_academy.sh -h`: Muestra información sobre las opciones disponibles para ejecutar el script.

**IMPORTANTE**: Tras ejecutar el script, se crea la carpeta `src`, que contiene todos los archivos de Robotics Application Manager (RAM). Desde esa carpeta, se pueden crear ramas y confirmar cambios en el repositorio de RAM. Para el resto de los cambios, también se puede trabajar normalmente desde la carpeta Robotics Academy, donde el contenido de la carpeta src, junto con los archivos repetitivos comunes, se ignora automáticamente. Y por último, cuando se quiera terminar de desarrollar, se cierra el script con Ctrl+C.

## 2.2 Problemas que pueden surgir

**PROBLEMA 1**: Instalación incorrecta de alguna dependencia o alguna ruta no agregada.

**PROBLEMA 2**: No se lanza el frontend.

### 2.2.1 Solución 1: Lanzar el frontend desde otra terminal

**TERMINAL 1**

`cd /RoboticsAcademy`

**TERMINAL 2**

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash`
`nvm install 20`
`nvm use 20`
`npm install --global yarn`
`cd react_frontend/ && yarn install && yarn run dev`

### 2.2.2 Solución 2: Generar un RADI (borrar la imagen generada y volver a hacerla)

`cd /scripts/RADI`: Accede al directorio en el que se encuentra el script.

`chmod +x build.sh`: Concede permisos al equipo para crear imágenes del Docker.

`./build.sh -h`: Muestra información adicional sobre el script `build.sh`.

`./build.sh -a [ROBOTICS_ACADEMY] -i [ROBOTICS_INFRASTRUCTURE] -m [RAM] -r [ROS_DISTRO] -t [IMAGE_TAG]`: Construye la imagen del Docker.

- **[ROBOTICS_ACADEMY]**: Nombre de la rama del repositorio de Robotics Academy que se usará (el valor predeterminado es humble-devel).

- **[ROBOTICS_INFRASTRUCTURE]**: Nombre de la rama del repositorio de Robotics Infrastructure que se utilizará (el valor predeterminado es "humble-devel").

- **[RAM]**: Nombre de la rama del repositorio Robotics Application Manager que se usará (el valor predeterminado es "humble-devel").

- **[ROS_DISTRO]**: Distribución de ROS que se debe usar, donde el script actual es compatible con humble (el valor predeterminado es "humble").

- **[IMAGE_TAG]**: Etiqueta de la imagen de Docker que se creará (el valor predeterminado es "test").

### 2.2.3 Solución 3: Buscar imágenes instaladas

`sudo docker images`: Muestra qué imágenes del Docker están instaladas.

`sudo docker rmi [image_id]`: Elimina imágenes del Docker ya instaladas.

`sudo docker ps -a`: Enumera todos los contenedores instalados.

`sudo docker rm [docker_id]`: Elimina todos los contenedores que utilizan la imagen que se desea eliminar.

## 2.3 Uso de Docker Run

```sh
cd common
cd console_interfaces
zip -r ../common.zip console_interfaces/
cd ..
cd gui_interfaces
zip -r -u ../common.zip gui_interfaces/
cd ..
cd hal_interfaces
zip -r -u ../common.zip hal_interfaces/
cd ../..
mv common/common.zip react_frontend/src/common.zip
```

```sh
docker run --hostname my-postgres --name academy_db -d\
    -e POSTGRES_DB=academy_db \
    -e POSTGRES_USER=user-dev \
    -e POSTGRES_PASSWORD=robotics-academy-dev \
    -e POSTGRES_PORT=5432 \
    -d -p 5432:5432 \
    jderobot/robotics-database:latest
```

`docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all" || echo "") --device /dev/dri -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Selección automática de GPU.

`docker run --rm -it --device /dev/dri -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Selección automática de GPU (sin NVIDIA).

`docker run --rm -it -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 -p 7164:7164 --link academy_db jderobot/robotics-academy:latest`: Sólo CPU.

## 2.4 Uso de NVIDIA

`sudo apt-get update`: Actualiza todos los paquetes.

`sudo apt-get install -y nvidia-docker2`: Instala el runtime de NVIDIA para usarlo como Docker.

`sudo systemctl restart docker`: Reinicia el servicio Docker para actualizar la nueva configuración.

`docker info | grep -i runtime`: Comprueba si Docker reconoce a NVIDIA como un nuevo entorno de ejecución (lo más probable es que no lo reconozca). Por ello, habrá que hacerlo manualmente editando y/o creando el fichero `/etc/docker/daemon.json` de la siguiente forma:

```sh
{
  "runtimes": {
    "nvidia": {
      "path": "nvidia-container-runtime",
      "runtimeArgs": []
    }
  }
}
```

`dpkg -l | grep nvidia-container-runtime`: Comprueba si nvidia-runtime está instalado.

`sudo apt-get install -y nvidia-container-runtime`: Instala nvidia-runtime.

`sudo systemctl restart docker`: Reinicia el servicio de Docker para actualizar la configuración y comprobar que todo funciona correctamente.

## 2.5 Agregar un nuevo ejercicio

### 2.5.1 Crear la carpeta de ejercicios con el código fuente y el frontend

```sh
mkdir exercises/static/exercises/exercise_id
cd exercises/static/exercises/exercise_id
```

```sh
mkdir python_template/<ros_version>
cd python_template/<ros_version>
touch WebGUI.py`: Interacciones entre el ejercicio y el frontend.
touch HAL.py`: Capa de abstracción de hardware para acceder a los datos del robot.
touch Frequency.py`: Control de frecuencia del código iterativo.
```

```sh
mkdir react-components
cd react-components
touch WebGUI.js: Interfaz de ejercicios.
```

### 2.5.2 Añadir el ejercicio a la base de datos

**IMPORTANTE**: Es necesario añadir el frontend a la carpeta `exercises/templates/exercises` y crear dicha entrada en `database/exercise/db.sql`.

**PASO 1**: Inicia el contenedor de forma normal.

**PASO 2**: Accede a `http://127.0.0.1:7164/admin/` en un navegador e inicia sesión con "usuario" y "contraseña".

**PASO 3**: Haz clic en "agregar ejercicio" y completa los campos obligatorios especificados.

**PASO 4**: Abre una shell en el contenedor universe_db: `docker exec -it universe_db bash`.

**PASO 5**: Vuelca los cambios usando `./scripts/saveDb.sh`.

Una entrada de ejercicio en la base de datos debe incluir los siguientes datos:

`exercise id`: Identificador único de ejercicio que debe coincidir con el nombre de la carpeta.

`name`: Nombre a mostrar en la lista de ejercicios.

`description`: Descripción a mostrar en la lista de ejercicios.

`tags`: Un ejercicio debe incluir al menos una etiqueta ROS ("ROS2"). El ejercicio solo se mostrará en la lista de ejercicios cuando la versión de ROS del Robotics Backend instalada aparezca en las etiquetas (la barra de búsqueda también utiliza etiquetas).

`status`: Cambia el indicador de estado (ACTIVO = verde; PROTOTIPO = amarillo; INACTIVO = rojo).

`url`: URL de la documentación del ejercicio.

## 2.6 Agregar cambios locales al Robotics Backend con cambios bidireccionales

Este método consiste en saber si se ha trabajado hasta ahora en su configuración local y busca importar todos los cambios dentro de Robotics Backend y al mismo tiempo poder editar y conservar cambios adicionales.

**PASO 1**: Desde la terminal, abre el directorio donde se encuentra el proyecto/código (`cd ~/my_project`).

**PASO 2**: Añade `-v $(pwd):/location_in_radi` al comando CLI `docker run` utilizado para ejecutar el contenedor (`docker run --rm -it $(nvidia-smi >/dev/null 2>&1 && echo "--gpus all" || echo "") --device /dev/dri -p 7164:7164 -p 6080:6080 -p 6081:6081 -p 1108:1108 -p 7163:7163 jderobot/robotics-backend -v $(pwd):/home jderobot/robotics-academy`).

Esto importará el directorio local dentro del contenedor de Docker. Si se ha utilizado el comando de ejemplo como el anterior, donde la ubicación donde se ejecuta el comando está montada en la carpeta de inicio dentro del contenedor de Docker, simplemente podrá ver todos los directorios locales montados dentro del `/home` del Robotics Backend.

Para asegurar que el directorio local se haya montado correctamente en la ubicación correcta dentro del Robotics Backend, accede a `http://localhost:1108/vnc.html` después de iniciar un ejercicio (esto implica hacer clic en el botón de inicio de cualquier ejercicio), lo que abrirá una instancia de consola vnc donde se puede verificar la integridad del montaje.

# 3. Guía REAL de instalación de Docker en Ubuntu 24.04

Se abre una terminal por defecto (desde el directorio HOME):

`cd Escritorio/`

`git clone --recurse-submodules https://github.com/JdeRobot/RoboticsAcademy.git`

`cd RoboticsAcademy/`

`./scripts/develop_academy.sh`

Al ejecutar este comando, aparece el siguiente error:

```sh
...
./scripts/develop_academy.sh: linea 146: yarn: orden no encontrada
./scripts/develop_academy.sh: linea 147: yarn: orden no encontrada
./scripts/develop_academy.sh: linea 166: docker: orden no encontrada
Cleaning up...
./scripts/develop_academy.sh: linea 28: docker: orden no encontrada
```

Una vez visto el error, se siguen ejecutando comandos:

```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

`sudo systemctl status docker`

`sudo apt install docker-compose`

`docker --version`

`sudo groupadd docker`

`sudo usermod -aG docker $USER`

`newgrp docker`

`sudo ./scripts/develop_academy.sh`

Cuando haya terminado el proceso de instalación, el Docker se queda ejecutando. Para ello, se detiene la ejecución con Ctrl+C para poder seguir con el proceso.

Una vez ejecutados todos estos comandos, hay que entrar en VSCode desde el directorio de RoboticsAcademy e instalar las siguientes extensiones:

IMAGEN DE LA EXTENSIÓN CONTAINER TOOLS

IMAGEN DE LA EXTENSIÓN PROJECT MANAGER

Una vez hecho todo esto, lo más conveniente es reiniciar el ordenador para que todos los cambios queden guardados correctamente. Esto se hace también con el objetivo de que todos los contenedores se carguen correctamente dentro de VSCode.

Ya reiniciado el ordenador, conviene asegurarse de que las imágenes de los contenedores (tanto de robotics-academy como de robotics-database) aparecen dentro del desplegable IMAGES al entrar en la opción Containers.

`cd Escritorio/RoboticsAcademy/`

`code .`

IMAGEN MENÚ DESPLEGABLE CONTAINERS

Y por último, se entra en la opción Project Manager y se hace clic en el directorio RoboticsAcademy. Una vez dentro del directorio, se sigue el siguiente bucle de trabajo:

IMAGEN MENÚ DESPLEGABLE PROJECT MANAGER

PASO 1: Hacer los cambios necesarios en RoboticsInfrastructure.

PASO 2: Subir dichos cambios a una nueva rama (Publish Branch).

PASO 3: Acceder al directorio RADI dentro del directorio scripts.

`cd scripts/RADI/`

PASO 4: Ejecutar el script build.sh dentro de la nueva rama.

`./build.sh -i <branch_name>`

PASO 5: Volver al directorio scripts.

`cd ..`

PASO 6: Cambiar la siguiente línea del fichero compose_cfg/dev_humble_cpu.yaml

```sh
# ANTES
image: jderobot/robotics-academy:latest

# DESPUÉS
image: jderobot/robotics-academy:test
```

PASO 7: Ejecutar el script de instalación para lanzar el docker con todos los cambios realizados.

`./scripts/develop_academy.sh`

PASO 8: Acceder a la dirección web `http://0.0.0.0:7164/` que aparece en la terminal al ejecutar el comando anterior para poder entrar a Unibotics en local y verificar que los cambios se han realizado correctamente.

NOTA: En caso de que los cambios se hayan realizado únicamente en el fichero database/universes.sql, sólo sería necesario realizar el último paso (PASO 8).

# 4. Modificaciones a llevar a cabo para migrar de Gazebo 11 a Gazebo Harmonic

Todos los ficheros que se van a ir enumerando a continuación se encuentran en el directorio de GitHub RoboticsInfrastructure dentro del directorio de GitHub RoboticsAcademy (todos ellos dentro de la rama humble-devel):

RoboticsAcademy: https://github.com/JdeRobot/RoboticsAcademy

RoboticsInfrastructure: https://github.com/JdeRobot/RoboticsInfrastructure/tree/humble-devel

A continuación se van a ir enumerando cada uno de los ficheros que habría que modificar para llevar a cabo esta tarea de migración de Gazebo 11 a Gazebo Harmonic, así como los cambios a realizar en cada uno de ellos:

* FICHERO RoboticsInfrastructure/Launchers/<exercise_name>/spawn_robot.launch.py

```sh
# CAMBIAR EL NOMBRE DEL ROBOT
model_folder = "<robot_name>"
```

```sh
# CAMBIAR EL NOMBRE DEL FICHERO .YAML
bridge_params = os.path.join(
    get_package_share_directory("custom_robots"), "params", "<robot_params.yaml>"
)
```

```sh
# AÑADIR / ELIMINAR LAS SIGUIENTES LÍNEAS SI EL ROBOT TIENE / NO TIENE CÁMARA
start_gazebo_ros_image_bridge_cmd = Node(
    package="ros_gz_image",
    executable="image_bridge",
    arguments=["/<robot_name>/camera/image_raw"],
    output="screen",
)

start_gazebo_ros_depth_bridge_cmd = Node(
    package="ros_gz_image",
    executable="image_bridge",
    arguments=["/<robot_name>/camera/depth"],
    output="screen",
)

ld.add_action(start_gazebo_ros_image_bridge_cmd)
ld.add_action(start_gazebo_ros_depth_bridge_cmd)
```

* FICHERO CMakeLists.txt

```sh
# AÑADIR LOS DIRECTORIOS CORRESPONDIENTES A LAS NUEVAS MIGRACIONES
install(
  DIRECTORY
    <exercise_name>/models
    <robot_name>/models
    <robot_name>/urdf
    <robot_name>/params
  DESTINATION share/${PROJECT_NAME})
```

* FICHERO RoboticsInfrastructure/Launchers/<exercise_name>.launch.py

```sh
# CAMBIAR RUTAS Y NOMBRES DE LOS FICHEROS CARGADOS
robot_launch_dir = "/opt/jderobot/Launchers/<exercise_name>"
robot_model_dir = os.path.join(package_dir, "models/<robot_name>")
world_file_name = "<exercise_name>.world"
```

* FICHERO RoboticsInfrastructure/Worlds/<exercise_name>.world

```sh
# MODIFICAR TODOS LOS APARTADOS CON EL FLAG INCLUDE

# GAZEBO 11
<include>
  <uri>model://house_int2</uri>
  <pose>0 0 0 0 0 0</pose>
</include>

# GAZEBO HARMONIC
<include>
  <name>house_int2</name>
  <uri>model://Lake house_int2</uri>
  <pose>0 0 0 0 0 0</pose>
</include>
```

* FICHERO RoboticsInfrastructure/CustomRobots/<robot_name>/models/<robot_name>/model.sdf

```sh
# MODIFICAR TODOS LOS APARTADOS CON EL FLAG SENSOR Y EL FLAG PLUGIN

# GAZEBO 11
<sensor name='laser' type='ray'>
  <ray>
    <scan>
      <horizontal>
        <samples>180</samples>
        <resolution>1.000000</resolution>
        <min_angle>-1.570000</min_angle>
        <max_angle>1.570000</max_angle>
      </horizontal>
    </scan>
    <range>
      <min>0.080000</min>
      <max>10.000000</max>
      <resolution>0.010000</resolution>
    </range>
  </ray>
  <update_rate>20.000000</update_rate>
  <always_on>1</always_on>
  <visualize>1</visualize>
  <plugin name="gazebo_ros_head_hokuyo_controller" filename="libgazebo_ros_ray_sensor.so">
    <ros>
      <remapping>~/out:=/roombaROS/laser/scan</remapping>
    </ros>
    <output_type>sensor_msgs/LaserScan</output_type>
    <frameName>laser</frameName>
    <update_rate>20.000000</update_rate>
  </plugin>
</sensor>

# GAZEBO HARMONIC
<sensor name="hls_lfcd_lds" type="gpu_lidar">
  <always_on>true</always_on>
  <visualize>true</visualize>
  <pose>-0.064 0 0.121 0 0 0</pose>
  <update_rate>5</update_rate>
  <topic>turtlebot3/laser/scan</topic>
  <gz_frame_id>base_scan</gz_frame_id>
  <lidar>
    <scan>
      <horizontal>
        <samples>360</samples>
        <resolution>1.000000</resolution>
        <min_angle>0.000000</min_angle>
        <max_angle>6.280000</max_angle>
      </horizontal>
    </scan>
    <range>
      <min>0.20000</min>
      <max>3.5</max>
      <resolution>0.015000</resolution>
    </range>
  </lidar>
</sensor>

<plugin filename="ignition-gazebo-odometry-publisher-system" name="ignition::gazebo::systems::OdometryPublisher">
  <odom_frame>odom</odom_frame>
  <robot_base_frame>base_footprint</robot_base_frame>
  <odom_publish_frequency>50</odom_publish_frequency>
  <odom_topic>turtlebot3/odom</odom_topic>
  <dimensions>3</dimensions>
</plugin>
```

* FICHERO RoboticsInfrastructure/CustomRobots/<robot_name>/params/robot_params.yaml

```sh
# AÑADIR LOS TOPICS DE TODOS LOS SENSORES QUE COMPONEN EL ROBOT

# gz topic published by DiffDrive plugin
- ros_topic_name: "turtlebot3/odom"
  gz_topic_name: "turtlebot3/odom"
  ros_type_name: "nav_msgs/msg/Odometry"
  gz_type_name: "gz.msgs.Odometry"
  direction: GZ_TO_ROS

# gz topic subscribed to by DiffDrive plugin
- ros_topic_name: "turtlebot3/cmd_vel"
  gz_topic_name: "turtlebot3/cmd_vel"
  ros_type_name: "geometry_msgs/msg/Twist"
  gz_type_name: "gz.msgs.Twist"
  direction: ROS_TO_GZ

# gz topic published by Sensors plugin (LIDAR)
- ros_topic_name: "turtlebot3/laser/scan"
  gz_topic_name: "turtlebot3/laser/scan"
  ros_type_name: "sensor_msgs/msg/LaserScan"
  gz_type_name: "gz.msgs.LaserScan"
  direction: GZ_TO_ROS

# gz topic published by Sensors plugin (Camera)
- ros_topic_name: "turtlebot3/camera/camera_info"
  gz_topic_name: "turtlebot3/camera/camera_info"
  ros_type_name: "sensor_msgs/msg/CameraInfo"
  gz_type_name: "gz.msgs.CameraInfo"
  direction: GZ_TO_ROS
```

* FICHERO RoboticsInfrastructure/database/universes.sql

```sh
# MODIFICAR LOS PARÁMETROS DE LOS MUNDOS

# MUNDO NO MIGRADO (ANTES)
25	Vacuums House Markers	/opt/jderobot/Launchers/marker_visual_loc.launch.py	None	ROS2	gazebo	{1,-1.5,0.6,0,0,0}

# MUNDO MIGRADO (DESPUÉS)
25	Vacuums House Markers	/opt/jderobot/Launchers/marker_visual_loc.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/marker_visual_loc.config"}	ROS2	gz	{1,-1.5,0.6,0,0,0}
```

# 5. Lanzamiento del RoboticsBackend en local

PASO 1: Para lanzar el RoboticsBackend y poder trabajar en Unibotics en local, se debe ejecutar el siguiente comando en la terminal:

`docker run --rm -it --device /dev/dri -p 6080:6080 -p 1108:1108 -p 7163:7163 jderobot/robotics-backend:latest`

PASO 2: Se deja ejecutando este comando en la terminal y se accede a la página web de Unibotics (https://unibotics.org/).

PASO 3: Una vez dentro de Unibotics, se selecciona el ejercicio en el que se quiera trabajar.

IMPORTANTE: Para que todo funcione correctamente, debe estar seleccionada la opción 'Local ROS2 (RoboticsBackend 4)', que aparece en la esquina superior derecha al hacer clic en el icono del nombre.

IMAGEN OPCIÓN ROBOTICSBACKEND LOCAL

PASO 4: Verificar que, tanto la simulación en Gazebo como la terminal han cargado correctamente.

IMAGEN UNIBOTICS LOCAL