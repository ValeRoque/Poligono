# PRACTICA - Polígono 2D en Blender con Python

## Introducción

En esta práctica se genera un polígono 2D en Blender utilizando Python y la API bpy.  
Los vértices se calculan matemáticamente usando coordenadas polares convertidas a coordenadas cartesianas.

La figura se construye en el plano XY manteniendo Z = 0 para que sea una figura bidimensional.

---

## Código en Python

```python
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    # Crear una nueva malla y un nuevo objeto
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)

    # Vincular el objeto a la escena actual
    bpy.context.collection.objects.link(objeto)

    vertices = []
    aristas = []

    # Cálculo de vértices usando coordenadas polares a cartesianas
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0))  # Z = 0 para mantenerlo en 2D

    # Definir las conexiones (aristas) entre los vértices
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))

    # Cargar los datos en la malla
    malla.from_pydata(vertices, aristas, [])
    malla.update()

# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

# Llamada a la función: Un hexágono de radio 5
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```

---

## Resultado en Blender

![Polígono en Blender](poligono_blender.png)

---

## Explicación

Se utilizan las fórmulas matemáticas:

x = r cos(θ)  
y = r sen(θ)

Donde:
- r es el radio
- θ es el ángulo que se calcula dividiendo 360° entre el número de lados

Esto permite distribuir los vértices uniformemente formando un hexágono regular.
