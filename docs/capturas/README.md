---
type: Evidencias
title: Índice de capturas
description: Qué muestra cada captura de docs/capturas y qué requisito evidencia.
timestamp: 2026-11-19
---

# Capturas

Capturas de la versión actual (27/10/2026 las de los Hitos y 29/10/2026 las del juego y DoneTonic), hechas con la aplicación real en local y datos de ejemplo, salvo las de `produccion/` (aplicación desplegada, 23/10/2026).

## Hitos 1–3 (`docs/capturas/app/`)

| Fichero | Qué muestra | Requisito |
|---|---|---|
| `01-inicio.png` | Página de inicio sin sesión: «Crear cuenta para jugar» / «Ya tengo cuenta», cómo se juega y estado del servidor | P7 |
| `02-registro-validacion.png` | Registro con email y contraseña no válidos: campos en rojo y errores explicados | P8, P17 |
| `03-entrar-error.png` | Inicio de sesión con contraseña incorrecta: «Email o contraseña incorrectos» | P8, P17 |
| `04-perfil.png` | Mi cuenta de un usuario normal: mote, avatar, email, rol, estado, alta, cerrar sesión y darse de baja | P9, P13 |
| `05-confirmar-baja.png` | Diálogo de confirmación antes de darse de baja | P13 |
| `06-usuario-sin-permiso.png` | Un usuario normal intenta abrir `#/admin` y vuelve a Mi cuenta con el aviso «Esa página es solo para administradores» | P10, P13 |
| `07-panel-admin.png` | Panel de administración: contadores, buscador y tabla con email, rol, estado, alta y acciones | P13 |
| `08-admin-busqueda.png` | Panel filtrado por email | P13 |
| `09-admin-sin-resultados.png` | Estado vacío de la búsqueda con «Limpiar búsqueda» | P17 |
| `10-admin-confirmar-eliminar.png` | Diálogo antes de eliminar otra cuenta | P13 |
| `11-panel-admin-movil.png` | Panel de administración en un móvil (tarjetas) | Responsive |
| `12-perfil-movil.png` | Mi cuenta en un móvil con el menú plegado | Responsive |
| `13-pagina-no-encontrada.png` | Ruta inexistente: «Fuera de juego» | P17 |

## Hito 4 (`docs/capturas/hito4/`)

| Fichero | Qué muestra | Requisito |
|---|---|---|
| `01-entrar-google.png` | Entrar con el botón «Entrar con Google» | P15 |
| `02-registro-correo-enviado.png` | Registro: aviso de cuenta creada y correo de confirmación enviado | P16 |
| `03-cuenta-pendiente-reenvio.png` | Login de una cuenta pendiente: error y botón para reenviar el correo | P16, P17 |
| `04-reenvio-hecho.png` | Aviso de reenvío (no revela si la cuenta existe) | P16 |
| `05-enlace-invalido.png` | Enlace manipulado: aviso rojo, la cuenta sigue pendiente | P16, P17 |
| `06-cuenta-confirmada.png` | Enlace correcto: aviso verde de cuenta confirmada | P16 |
| `07-primer-acceso.png` | Primer acceso tras confirmar: Mi cuenta con el aviso de bienvenida | P16 |
| `08-google-cuenta-creada.png` | Cuenta creada con Google: Mi cuenta con el aviso «Cuenta creada con tu cuenta de Google» | P15 |
| `09-error-con-reintento.png` | Fallo del servidor al cargar el panel: «No hemos podido cargarlo» con Reintentar | P17 |
| `10-panel-admin-estados.png` | Panel con cuentas activas, pendientes, eliminadas, locales y de Google | P13, P15, P16 |
| `11-privacidad.png` | Política de privacidad | T26 |

## Producción (`docs/capturas/produccion/`)

| Fichero | Qué muestra |
|---|---|
| `01-correo-confirmacion.png` | Correo de confirmación real recibido en Gmail (enlace tapado) |
| `02-actions-verde.png` | GitHub Actions: CI y despliegue en Cloud Run en verde tras los PRs del Hito 4 |
| `03-google-publicada.png` | Google Auth Platform: aplicación publicada («En producción», usuarios externos) |

## Sprint 2: el juego (`docs/capturas/juego/`)

| Fichero | Qué muestra | Requisito |
|---|---|---|
| `01-menu.png` | Menú del juego (inicio con sesión): el jugador paseando, la ficha del aficionado, tus tiempos y el menú | RJ-01 |
| `02-vestuario.png` | Vestuario: lista de jugadores con filtros, ficha con estadísticas y habilidades, y equipaciones | RJ-04, RJ-18 |
| `03-partida.png` | Partida en curso: mundo procedural, rivales, armas automáticas y HUD | RJ-02, RJ-06, RJ-08 |
| `04-mejora.png` | Subida de nivel: elegir una de tres mejoras | RJ-02, RJ-05 |
| `05-jefe.png` | Un jefe (Vinicius Jr.) con su barra de vida | RJ-07 |
| `06-fin.png` | Fin de la partida: tiempo, estadísticas, daño por habilidad y logro nuevo | RJ-18, RJ-19 |
| `07-entrenamiento.png` | Entrenamiento con el botón del modo pruebas (administrador) | RJ-13, RJ-16 |
| `08-contratar.png` | Mercado de fichajes con las fichas de contrato | RJ-12 |
| `09-logros.png` | Vitrina de logros con su dato de historia | RJ-18 |
| `10-combinaciones.png` | Combinaciones y ultras con lo que pide cada una | RJ-15 |
| `11-direccion.png` | Dirección: elegir entrenador y su ulti | RJ-04 |
| `12-ranking.png` | Ranking de la grada (mote y avatar, nunca el correo) | RJ-14, RJ-17 |
| `13-identidad.png` | Tu ficha: mote y avatar | RJ-17 |
| `14-ajustes.png` | Ajustes de sonido, gráficos y controles | RJ-10, RJ-20 |
| `15-pausa.png` | Menú de pausa | RJ-10 |
| `16-final-copa.png` | Ambiente de la Final de la Copa del Rey (a partir del minuto 5) | RJ-09 |

## Fase 7 (`docs/capturas/fase7/`)

| Fichero | Qué muestra |
|---|---|
| `01-sprites-antes.png` · `02-sprites-despues.png` | Todos los muñecos (jugadores, entrenadores, jefes y rivales) antes y después de la Fase 7 |
| `03-menu.png` · `04-menu-movil.png` | Menú nuevo (translúcido, botón de entrada) en escritorio y en el móvil |
| `05-mapa-ambientes.png` | Los cinco ambientes del mapa, antes (arriba) y después (abajo) |
| `06-partida.png` · `07-jefe.png` | Partida y un jefe con el pixel art nuevo |
| `08-habilidades-nuevas.png` · `09-efectos.png` | Fuera de juego y presión alta; efectos de las habilidades de disparo |
| `10-combinaciones.png` | Pantalla de combinaciones y ultras |
| `11-pausa.png` · `12-pausa-salir.png` | Pausa con el árbol de habilidades → combinaciones → ultras, y la confirmación al salir |

## DoneTonic (`docs/capturas/donetonic/`, 29/10/2026)

| Fichero | Qué muestra |
|---|---|
| `01-tablero-sprint.png` | Tablero del Sprint 1 al 100 % |
| `02-graficas-sprint.png` | Sprint Burndown y Burnup del Sprint 1 |
| `03-backlog.png` | Product Backlog y los dos sprints |
| `04-tablero-sprint2.png` | Tablero del Sprint 2 al 100 % con las etiquetas de fase |
| `05-sprint2-tareas-hechas.png` | Tareas de P48 en la columna Hecho (50 tareas en total) |
| `06-graficas-sprint2.png` | Sprint Burndown y Burnup del Sprint 2 |
| `07-graficas-backlog.png` | Backlog Burndown y Burnup de los dos sprints |
| `08-pbi-detalle.png` | Detalle de P44: historia de usuario, criterios de aceptación y tareas |

## Otras

| Carpeta | Contenido |
|---|---|
| `docs/capturas/prueba-tecnica/` | Respuestas de `POST /api/prueba/dominio` en producción (permitido y subdominio rechazado) |
