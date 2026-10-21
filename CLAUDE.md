# CLAUDE.md — Tablero de Proyectos (tablero-proyectos)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
`index.html` es el tablero de proyectos de automatización e IA del Área de Datos e IA, por área de la empresa; mide sobre todo las **horas ahorradas**.
- **Dashboard Ejecutivo** (entrada, de solo lectura para todos): "Productividad ganada" en h/semana, con equivalencias (h/mes = × 4.33; personas equivalentes = ÷ 48), avance, proyectos, terminados, en aplicación, no logrados, entrega promedio y modificaciones por proyecto. Paneles de impacto por área, estados, resumen de entrega, top 8 por impacto, avance por área, "En curso ahora" y "No logrados · motivo". Filtro "Ver por área", descarga como HTML e imprimir.
- **"Mis registros"** (solo quien registra): kanban de 4 columnas (Por hacer, En aplicación, Terminado, No logrado) con arrastrar y soltar. Pasar a No logrado exige un motivo; pasar a Terminado deja el avance en 100.
- Proyecto: título, área, estado, prioridad, avance (0–100), frecuencia (diaria, semanal, quincenal, mensual, única), horas por vez, veces modificado, motivo y notas. Ahorro semanal = horas por vez × factor (diaria 6, semanal 1, quincenal 0.5, mensual 0.23, única 0).
- Áreas: se agregan, renombran (los proyectos siguen), cambian de color y se eliminan solo si no tienen proyectos.
- Reporte imprimible; Excel LIVES.

## Base de datos (Supabase lives-datos)
- Leer: `tablero_leer` (proyectos, áreas y permiso de la persona).
- Guardar: cada cambio sube solo, 0,8 s después, con `tablero_guardar_proyecto`, `tablero_borrar_proyecto` (borrado real; la pantalla sugiere usar "No logrado") y `tablero_guardar_areas`. Si falla, el cambio queda en cola con "Reintentar guardado"; el navegador avisa antes de cerrar con cambios pendientes.
- `tablero_reemplazar_todo` reemplaza todo en un paso y antes guarda una copia en `tablero_respaldos`. Hoy solo lo usa la importación de un JSON, que no tiene botón.
- Tablas: `tablero_proyectos`, `tablero_areas`, `tablero_acceso` (quién ve y quién edita), `tablero_respaldos` (copias, no se borran).
- No usa Storage.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 1 persona (Datos e IA).

## Quién puede editar
- Lo decide la base: `tablero_permiso` ('ver' o 'editar', con la tabla `tablero_acceso`); la pantalla solo edita con 'editar' y fuera de modo supervisión. Sin permiso, "Mis registros · solo para quien registra".
- A Sistemas: `tablero_permiso` es la pieza que reemplazan con su seguridad.

## Pendientes vistos al escribir esto (21/10/2026)
- El aviso de "copiar link de observador" nombra a dos personas con texto fijo; debe pasar a texto genérico. Ese botón está dentro de una ventana que hoy no se abre.
- Código sin uso: importar y exportar JSON, `esModoObservador()`, `initObservador()` y `openDash()`.
