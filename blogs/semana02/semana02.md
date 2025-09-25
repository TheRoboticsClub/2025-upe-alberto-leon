# 1. Ejercicio de calentamiento: Cambiar mundo

Para irme familiarizando con el software de RoboticsInfrastructure, se me ha pedido como primera tarea cambiar el mundo de Gazebo de uno de los ejercicios que ya se han migrado a Gazebo Harmonic.

En primer lugar, he accedido al fichero RoboticsInfrastructure/database/universes.sql para localizar aquellos mundos ya migrados a Gazebo Harmonic.

**IMPORTANTE**: La forma más sencilla de identificar los mundos migrados a Gazebo Harmonic, es mirando si en su columna "type" aparece escrito "gazebo" o "gz". En caso de que aparezca escrito "gazebo", significa que ese mundo todavía se encuentra en Gazebo 11 y no se ha migrado aún. Pero si aparece escrito "gz", significa que ese mundo ya se ha migrado a Gazebo Harmonic.

A continuación se listan todos los mundos que tienen escrito "gz" en su columna "type", y que podrían utilizarse para llevar a cabo este ejercicio:

```sh
12	Laser Mapping Warehouse	/opt/jderobot/Launchers/laser_mapping.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/laser_mapping.config"}	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
25	Vacuums House Markers	/opt/jderobot/Launchers/marker_visual_loc.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/marker_visual_loc.config"}	ROS2	gz	{1,-1.5,0.6,0,0,0}
31	Rescue People Harmonic	/opt/jderobot/Launchers/rescue_people.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/rescue_people.config"}	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
32	Follow Road Harmonic	/opt/jderobot/Launchers/follow_road.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/follow_road.config"}	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
33	Small Laser Mapping Warehouse	/opt/jderobot/Launchers/small_laser_mapping.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/small_laser_mapping.config"}	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
34	Pick And Place Arm	/home/dev_ws/src/IndustrialRobots/ros2_SimRealRobotControl/ros2srrc_launch/moveit2/moveit2.launch.py	None	ROS2	gazebo	{0.0,0.0,0.0,0.0,0.0,0.0}
35	Car Junction	/opt/jderobot/Launchers/car_junction.launch.py	{"gzsim":"/opt/jderobot/Launchers/visualization/car_junction.config"}	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
36	Drone Gymkhana Harmonic	/opt/jderobot/Launchers/drone_gymkhana.launch.py	None	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
37	Tower Inspection Harmonic	/opt/jderobot/Launchers/power_tower_inspection.launch.py	None	ROS2	gz	{0.0,0.0,0.0,0.0,0.0,0.0}
```

En mi caso, he seleccionado los mundos correspondientes a los ejercicios de "Laser Mapping" (número 12) y "Marker Based Visual Loc" (número 25). El mundo del "Laser Mapping" es un almacén tipo Amazon, mientras que el mundo del "Marker Based Visual Loc" es una casa de dos plantas.

El resultado final de este ejercicio visualizará el almacén del "Laser Mapping" en la ventana de Gazebo del ejercicio "Marker Based Visual Loc".

A continuación se muestra cómo se ve inicialmente el ejercicio "Marker Based Visual Loc" al lanzar el Docker de RoboticsAcademy antes de realizar cualquier cambio en el código.

IMAGEN EJERCICIO MARKER BASED VISUAL LOC ORIGINAL

El único cambio que he realizado ha sido en el fichero RoboticsInfrastructure/Launchers/marker_visual_loc.launch.py en la siguiente línea:

```sh
# ANTES (MUNDO ORIGINAL)
world_file_name = "marker_visual_loc.world"

# DESPUÉS (MUNDO NUEVO)
world_file_name = "laser_mapping.world"
```

Una vez realizado este cambio, he hecho commit y push en una nueva rama que he creado llamada "test-aleon2020". Es importante hacer esto siempre para así evitar cualquier tipo de conflicto con la rama principal.

Como los cambios se han realizado en un fichero que se encuentra dentro de RoboticsInfrastructure, es obligatorio generar un nuevo RADI. Para ello, se deben ejecutar los siguientes comandos en la terminal:

```sh
cd scripts/RADI/
./build.sh -i test-aleon2020
```

**IMPORTANTE**: Si es la primera vez que se ejecuta este script, el tiempo que tardará en ejecutarse por completo será considerablemente largo, ya que debe configurar todo el entorno. Si es la primera vez que se ejecuta tardará unos 35-45 minutos, de lo contrario, tardará unos 3-4 minutos.

Una vez finalizada la ejecución del script build.sh, se debe regresar al repositorio principal:

```sh
cd ../..
```

Pero antes de ejecutar el script develop_academy.sh, hay que modificar la siguiente línea del fichero RoboticsAcademy/compose_cfg/dev_humble_cpu.yaml:

```sh
# ANTES
  robotics-academy:
    image: jderobot/robotics-academy:latest

# DESPUÉS
  robotics-academy:
    image: jderobot/robotics-academy:test
```

Con este cambio ya realizado, ya se puede lanzar el script develop_academy.sh:

```sh
./scripts/develop_academy.sh
```

Y por último, sólo quedaría acceder a la dirección web `http://0.0.0.0:7164/` que aparece en la terminal al ejecutar el comando anterior para poder entrar a Unibotics en local y verificar que los cambios se han realizado correctamente.

A continuación se muestra cómo se ve el ejercicio "Marker Based Visual Loc" al lanzar el Docker de RoboticsAcademy con todos estos cambios realizados en el código.

IMAGEN EJERCICIO MARKER BASED VISUAL LOC MODIFICADO