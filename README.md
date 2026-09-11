# Maxxor — Control de Personal en Tiempo Real

Panel de monitoreo de personal para equipos remotos o híbridos: sesiones de entrada/salida, capturas periódicas, actividad de navegación agrupada por sitio, y seguimiento de puntualidad contra horarios configurables.

Extraído como pieza de portfolio de un sistema de gestión más grande ([VirtuallCorp — Sistema de Ventas](../sistema-ventas-virtualcorp-github)), donde vive como uno de sus módulos.

## Demo standalone

Este repo es 100% autocontenido: **abre `index.html` en el navegador y funciona**, sin backend, sin base de datos, sin configuración. Los datos (empleados, sesiones, capturas, retrasos) se generan de forma ficticia en el propio navegador al cargar la página — no hay conexión a ningún servidor real.

## Qué muestra

- **KPIs en vivo**: en línea ahora, ingresos del día, capturas, tiempo de ocio, duración promedio de sesión
- **Panel "En línea ahora"**: tarjetas por empleado con estado, hora de entrada y tiempo conectado
- **Detalle por empleado**, con 4 pestañas:
  - *Historial* — sesiones del día (entrada, salida, duración, ocio)
  - *Capturas* — capturas periódicas de pantalla
  - *Actividad* — sitios visitados, agrupados y en orden cronológico
  - *Retrasos* — comparación contra el horario asignado, agrupado por mes, con tolerancia configurable
- **Configuración de horario por empleado**, con un selector de hora tipo reloj analógico dibujado en `<canvas>`

## Cómo funciona la detección de sesiones y retrasos

El sistema registra eventos atómicos (`checkin`, `checkout`, `captura`, `actividad`, `ocio_inicio`, `ocio_fin`) con marca de tiempo, y reconstruye sesiones de trabajo agrupándolos por empleado. Si un empleado deja de emitir eventos por más de 10 minutos sin un `checkout` explícito, se considera su última actividad como salida estimada — así el panel sigue siendo preciso incluso ante cierres de sesión inesperados (cierre de pestaña, corte de conexión, etc.).

Los retrasos se calculan comparando la hora real del primer `checkin` del día contra el horario configurado para ese día de la semana, con una tolerancia en minutos configurable por empleado.

## Stack técnico

- JavaScript vanilla (sin framework, sin build step)
- En el sistema original: [Supabase](https://supabase.com) (Postgres + tiempo real) como backend — en esta demo, reemplazado por datos generados en el navegador
- `<canvas>` para el selector de hora dibujado a mano

> Repo de portfolio: no contiene datos reales de ninguna persona ni empresa.
