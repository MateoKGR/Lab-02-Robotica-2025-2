# Lab-02-Robotica-2025-2
Laboratorio 2 de Robótica 2025-2s, realizado por Jeison Diaz y Mateo Ramos

# Integrantes
1. Jeison Nicolás Diaz Arciniegas [jediazar@unal.co](JeisonD0819)
2. Mateo Ramos Cujer [mramoscu@unal.edu.co](MateoKGR)

# Informe

Indice:
1. [Cuadro comparativo](#cuadro-comparativo)
2. [Descripción de las configuraciones home1 y home2](#descripcion-config)
3. [Procedimiento detallado](#procedimiento)
4. [Explicación completa](#explicacion)
5. [Descripción funcionalidades RoboDK](#descripcion-funciones)
6. [Análisis comparativo RoboDK y RobotStudio](#analisis)
7. [Diagrama de flujo](#diagrama)
8. [Plano de planta](#planos)
9. [Código desarrollado](#codigo)
10. [Videos simulación e implementación](#videos)

## Cuadro comparativo
## Descripción de las configuraciones home1 y home2
## Procedimiento detallado
La idea de empezar a lograr la parte de los robots es poder intentar
## Explicación completa
## Descripción funcionalidades RoboDK
## Análisis comparativo RoboDK y RobotStudio
## Diagrama de flujo
## Plano de planta
## Código desarrollado

```python

from robodk.robolink import *    # API para comunicarte con RoboDK
from robodk.robomath import *    # Funciones matemáticas
import math

#------------------------------------------------
# 1) Conexión a RoboDK e inicialización
#------------------------------------------------
RDK = Robolink()

# Elegir un robot (si hay varios, aparece un popup)
robot = RDK.ItemUserPick("Selecciona un robot", ITEM_TYPE_ROBOT)
#if not robot.Valid():
#    raise Exception("No se ha seleccionado un robot válido.")

# Conectar al robot físico
#if not robot.Connect():
#    raise Exception("No se pudo conectar al robot. Verifica que esté en modo remoto y que la configuración sea correcta.")

# Confirmar conexión
#if not robot.ConnectedState():
#    raise Exception("El robot no está conectado correctamente. Revisa la conexión.")

print("Robot conectado correctamente.")

#------------------------------------------------
# 2) Cargar el Frame (ya existente) donde quieres dibujar
#------------------------------------------------
frame_name = "Frame_from_Target1"
frame = RDK.Item(frame_name, ITEM_TYPE_FRAME)
home = RDK.Item('Target_Home', ITEM_TYPE_TARGET)
m1 = frame.Pose()

if not frame.Valid():
    raise Exception(f'No se encontró el Frame "{frame_name}" en la estación.')

robot.setPoseFrame(frame)
robot.setPoseTool(robot.PoseTool())
robot.setSpeed(300)   # mm/s - Ajusta según necesites
robot.setRounding(5)  # blending (radio de curvatura)

#------------------------------------------------
# 3) Parámetros de la figura (cardioide)
#------------------------------------------------
num_points = 200
a = 100
offset_x = 300
offset_y = 0
z_surface = 0
z_safe = 50
offsetN = 50

robot.MoveJ(home)

#------------------------------------------------
# 4) Movimiento al centro en altura segura
#------------------------------------------------
robot.MoveJ(transl(0, 0, z_surface + z_safe))
robot.MoveL(transl(0, 0, 30))

#------------------------------------------------
# 5) Dibujar la figura cardioide
#------------------------------------------------
full_turn = 2 * math.pi

for i in range(num_points + 1):
    t = i / num_points
    theta = full_turn * t
    r = a * (1 + math.cos(theta))
    x = r * math.cos(theta)
    y = r * math.sin(theta)
    robot.MoveL(transl(x + offset_x, y + offset_y, z_surface))

robot.MoveL(transl(x + offset_x, y + offset_y, z_surface + z_safe))

#------------------------------------------------
# 6) Letras adicionales (JEISON y MATEO)
#------------------------------------------------
point1 = transl(-20 + offsetN, -180, 0)
point2 = transl(0 + offsetN, -200, 0)

# Figura J
robot.MoveL(transl(50 + offsetN, -200, 0))
robot.MoveL(transl(50 + offsetN, -160, 0))
robot.MoveL(transl(0 + offsetN, -160, 0))
robot.MoveL(transl(-15 + offsetN, -160, 0))
robot.MoveL(transl(-15 + offsetN, -190, 0))
robot.MoveL(transl(0 + offsetN, -190, 0))
robot.MoveL(transl(0 + offsetN, -190, 50))

# Figura E
robot.MoveL(transl(0 + offsetN, -200, 50))
robot.MoveL(transl(50 + offsetN, -100, 0))
robot.MoveL(transl(50 + offsetN, -140, 0))
robot.MoveL(transl(20 + offsetN, -140, 0))
robot.MoveL(transl(20 + offsetN, -100, 0))
robot.MoveL(transl(20 + offsetN, -140, 0))
robot.MoveL(transl(-20 + offsetN, -140, 0))
robot.MoveL(transl(-20 + offsetN, -100, 0))
robot.MoveL(transl(-20 + offsetN, -100, 50))

# Figura I
robot.MoveL(transl(50 + offsetN, -80, 0))
robot.MoveL(transl(-20 + offsetN, -80, 0))
robot.MoveL(transl(-20 + offsetN, -80, 50))

# Figura S
robot.MoveL(transl(50 + offsetN, -20, 0))
robot.MoveL(transl(50 + offsetN, -60, 0))
robot.MoveL(transl(20 + offsetN, -60, 0))
robot.MoveL(transl(20 + offsetN, -20, 0))
robot.MoveL(transl(-20 + offsetN, -20, 0))
robot.MoveL(transl(-20 + offsetN, -60, 0))
robot.MoveL(transl(-20 + offsetN, -60, 50))

# Figura O
robot.MoveL(transl(50 + offsetN, 0, 0))
robot.MoveL(transl(50 + offsetN, 40, 0))
robot.MoveL(transl(-20 + offsetN, 40, 0))
robot.MoveL(transl(-20 + offsetN, 0, 0))
robot.MoveL(transl(50 + offsetN, 0, 0))
robot.MoveL(transl(50 + offsetN, 0, 50))

# Figura N
robot.MoveL(transl(-20 + offsetN, 60, 0))
robot.MoveL(transl(50 + offsetN, 60, 0))
robot.MoveL(transl(-20 + offsetN, 100, 0))
robot.MoveL(transl(50 + offsetN, 100, 0))
robot.MoveL(transl(50 + offsetN, 100, 50))

# ======================
# MATEO
# ======================

# Figura M
robot.MoveL(transl(-110 + offsetN, -200, 0))
robot.MoveL(transl(-40 + offsetN, -200, 0))
robot.MoveL(transl(-70 + offsetN, -160, 0))
robot.MoveL(transl(-40 + offsetN, -120, 0))
robot.MoveL(transl(-110 + offsetN, -120, 0))
robot.MoveL(transl(-110 + offsetN, -120, 50))

# Figura A
robot.MoveL(transl(-110 + offsetN, -100, 0))
robot.MoveL(transl(-40 + offsetN, -60, 0))
robot.MoveL(transl(-110 + offsetN, -20, 0))
robot.MoveL(transl(-110 + offsetN, -20, 20))
robot.MoveL(transl(-70 + offsetN, -80, 0))
robot.MoveL(transl(-70 + offsetN, -40, 0))
robot.MoveL(transl(-70 + offsetN, -40, 50))

# Figura T
robot.MoveL(transl(-110 + offsetN, 20, 0))
robot.MoveL(transl(-40 + offsetN, 20, 0))
robot.MoveL(transl(-40 + offsetN, 0, 0))
robot.MoveL(transl(-40 + offsetN, 40, 0))
robot.MoveL(transl(-40 + offsetN, 40, 50))

robot.MoveL(transl(-110 + offsetN, 80, 0))
robot.MoveL(transl(-40 + offsetN, 80, 0))
robot.MoveL(transl(-40 + offsetN, 60, 0))
robot.MoveL(transl(-40 + offsetN, 100, 0))
robot.MoveL(transl(-40 + offsetN, 100, 50))

# Finalización
robot.MoveJ(transl(0, 0, z_surface + z_safe))
robot.MoveJ(home)

print(f"¡Figura (rosa polar) completada en el frame '{frame_name}'!")

```

## Videos simulación e implementación