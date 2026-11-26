# Bernabeu Store - Control de Inventario y Ventas (MASM x86)

Autor: Jesus Eduardo Diaz Gastelum y Alezander Gonzalez Can

Sistema de consola hecho en ensamblador x86 (MASM) con `Irvine32` para llevar el inventario, ventas, resumen y persistencia en disco de **Bernabeu Store**.

## Descripcion del sistema

El programa funciona mediante un menu principal para operar la tienda en consola:

1. Registrar producto (validando codigo y evitando duplicados).
2. Ver inventario.
3. Buscar por codigo.
4. Recibir mercancia.
5. Realizar venta.
6. Mostrar resumen.
7. Guardar.
8. Salir.

### Regla adicional de negocio

Cada vez que se registra una venta y el stock restante del articulo queda menor o igual a 5 unidades, el sistema muestra en consola el aviso de stock bajo para pedir reposicion.

## Estructura de datos y archivo binario

- **En memoria:** cada registro de producto ocupa 40 bytes con sus campos de codigo, nombre/descripcion, precio/valores numericos y existencias.
- **En disco (`inventario_datos.dat`):** se guarda en formato binario iniciando con la firma `INV1`, el numero de registros guardados y los bloques de datos de cada producto.

## Decisiones tecnicas y referencias

Se opto por registros de tamano fijo en memoria (40 bytes) para simplificar el recorrido y calculo de desplazamientos en ensamblador, y por un encabezado `INV1` en el archivo `.dat` para validar el formato antes de cargar datos.

- Documentacion de Microsoft Macro Assembler (MASM) y herramientas de compilacion en Visual Studio.
- Referencia de procedimientos de entrada, salida y manejo de archivos de la libreria `Irvine32` (Kip Irvine).

## Pruebas realizadas

| Caso | Accion | Entrada de prueba | Resultado esperado | Resultado |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Alta de producto | Opcion 1 con codigo nuevo y datos validos | Guarda el producto en el arreglo | Pasa |
| 2 | Duplicado | Opcion 1 usando un codigo ya registrado | Rechaza el alta e informa duplicado | Pasa |
| 3 | Datos invalidos | Opcion 5 con cantidad `0`/negativa o codigo inexistente | Marca error y no modifica existencias | Pasa |
| 4 | Venta con stock <= 5 | Opcion 5 (`RMA-001`, cantidad `3`) dejando stock <= 5 | Descuenta piezas, calcula total y muestra `[AVISO: Quedan pocas unidades en existencia]` | Pasa |
| 5 | Resumen general | Opcion 6 | Muestra total de piezas, valor del inventario e ingresos de la sesion | Pasa |
| 6 | Guardar en disco | Opcion 7 | Escribe `inventario_datos.dat` y muestra confirmacion | Pasa |
| 7 | Salir | Opcion 8 | Cierra el programa con codigo 0 | Pasa |

## Compilacion y ejecucion

1. Abrir el proyecto en Visual Studio (`PapeleriaControlU2.slnx` / `PapeleriaControlU2.vcxproj`).
2. Configurar la solucion en **x86 (32 bits)** y comprobar que `Irvine32.inc` e `Irvine32.lib` esten configurados en las rutas de inclusion y enlace del proyecto.
3. Verificar que `main.asm` forme parte del proyecto.
4. Compilar la solucion (`Ctrl + Shift + B`) y ejecutar (`Ctrl + F5`).

La evidencia de compilacion y ejecucion en consola se incluye en `Evidencia_Compilacion_Bernabeu_Store`.
