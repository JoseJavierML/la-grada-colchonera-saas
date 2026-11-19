---
type: Decisiones
title: Decisiones técnicas y de proceso
description: Decisiones de diseño, de proceso y de producto tomadas durante los dos sprints, con las alternativas descartadas, el motivo y su estado.
timestamp: 2026-11-19
---

# Decisiones

Cada decisión indica qué se decidió, qué alternativas había, por qué y si sigue vigente. Las de diseño están reflejadas en [arquitectura.md](arquitectura.md); las de proceso, en [procesos.md](procesos.md).

## D-01 — Express 5 sin framework de frontend

**Qué se decidió:** Express 5 sin framework de frontend.

**Alternativas:** React, Vue, Next.js.

**Por qué:** Proyecto individual y didáctico: las capas se ven como ficheros, no hay paso de compilación, el backend sirve el frontend (sin CORS) y la frontera interfaz/cliente que pide el Hito 2 queda en dos ficheros. Express 5 pasa los errores de las funciones `async` al manejador común.

**Estado:** vigente.

## D-02 — Repositorio con dos implementaciones (memoria y PostgreSQL)

**Qué se decidió:** Repositorio con dos implementaciones (memoria y PostgreSQL).

**Alternativas:** Solo PostgreSQL; un ORM.

**Por qué:** Se puede arrancar y probar sin base de datos, y las **mismas pruebas** se pasan a las dos, lo que demuestra que la lógica no depende del almacenamiento (patrón Repository).

**Estado:** vigente.

## D-03 — Inyección de dependencias por parámetro

**Qué se decidió:** Inyección de dependencias por parámetro.

**Alternativas:** Módulos globales, singletons.

**Por qué:** Cada servicio recibe sus dependencias (`userRepository`, `passwordHasher`, `mailer`, `tokenSigner`, reloj, azar), así que las pruebas usan versiones falsas sin trucos.

**Estado:** vigente.

## D-04 — Sesiones en el servidor (`express-session` + PostgreSQL)

**Qué se decidió:** Sesiones en el servidor (`express-session` + PostgreSQL).

**Alternativas:** JWT en el navegador.

**Por qué:** Cerrar sesión o eliminar una cuenta la invalida al momento; la cookie es `httpOnly` y no contiene datos; las sesiones sobreviven a los reinicios y despliegues.

**Estado:** vigente.

## D-05 — Bajas lógicas (`status = deleted`) con el email libre

**Qué se decidió:** Bajas lógicas (`status = deleted`) con el email libre.

**Alternativas:** Borrar la fila; bloquear el email para siempre.

**Por qué:** Se conserva el historial y la auditoría; la cuenta deja de poder entrar y su sesión deja de valer, pero su email y su mote quedan libres para que la persona pueda volver con una cuenta nueva (índice único parcial).

**Estado:** vigente.

## D-06 — Token de confirmación firmado (HMAC) sin guardar en la base de datos

**Qué se decidió:** Token de confirmación firmado (HMAC) sin guardar en la base de datos.

**Alternativas:** Token aleatorio en una tabla.

**Por qué:** No hace falta tabla ni limpieza; caduca solo; no se puede fabricar ni modificar sin el secreto.

**Estado:** vigente.

## D-07 — OAuth 2.0 escrito con `fetch`

**Qué se decidió:** OAuth 2.0 escrito con `fetch`.

**Alternativas:** Passport.js u otra librería.

**Por qué:** El flujo es corto y se entiende entero (`state`, intercambio del código, email verificado); menos dependencias y fácil de probar con un cliente falso.

**Estado:** vigente.

## D-08 — Primer administrador por variables de entorno

**Qué se decidió:** Primer administrador por variables de entorno.

**Alternativas:** Script SQL, insertar a mano.

**Por qué:** No deja contraseñas en el código ni en scripts; es idempotente (si ya existe, no se toca).

**Estado:** vigente.

## D-09 — Migraciones SQL propias

**Qué se decidió:** Migraciones SQL propias.

**Alternativas:** ORM con migraciones.

**Por qué:** Pocas tablas; SQL legible y versionado, con un registro de las aplicadas.

**Estado:** vigente.

## D-10 — Cloud Run como principal y Render como alternativo

**Qué se decidió:** Cloud Run como principal y Render como alternativo.

**Alternativas:** Solo uno.

**Por qué:** Cloud Run aprovecha los créditos de educación, escala a cero y permite Secret Manager y acceso sin claves (WIF); Render fue el primer despliegue y queda de respaldo.

**Estado:** vigente.

## D-11 — Reglas del juego compartidas cliente/servidor

**Qué se decidió:** Reglas del juego compartidas cliente/servidor.

**Alternativas:** Reglas duplicadas.

**Por qué:** El servidor importa `club.js`, `config.js` y `logros.js`: una sola fuente de verdad para fichajes, entrenamiento y logros.

**Estado:** vigente.

## D-12 — Concurrencia optimista en el perfil del juego (`version`)

**Qué se decidió:** Concurrencia optimista en el perfil del juego (`version`).

**Alternativas:** Bloqueos, transacciones largas.

**Por qué:** Dos peticiones simultáneas no pueden gastar dos veces la misma ficha; la segunda se reintenta con los datos nuevos.

**Estado:** vigente.

## D-13 — Simulación del juego pura y determinista

**Qué se decidió:** Simulación del juego pura y determinista.

**Alternativas:** Lógica mezclada con el dibujo.

**Por qué:** Paso fijo de 1/60 s y azar con semilla: se prueba en Node sin navegador y el resultado no depende de los FPS.

**Estado:** vigente.

## D-14 — Gráficos y sonido generados con código

**Qué se decidió:** Gráficos y sonido generados con código.

**Alternativas:** Imágenes y ficheros de audio.

**Por qué:** Sin problemas de derechos (nada oficial), carga instantánea y fácil de variar (equipaciones, selecciones, intensidad de la música).

**Estado:** vigente.

## D-15 — Hacer un juego en lugar de una red social

**Qué se decidió:** Hacer un juego en lugar de una red social.

**Alternativas:** Red social tipo Twitter para aficionados (idea inicial).

**Por qué:** Cerrado el Sprint 1, el profesor dejó libertad para la lógica del producto. Un juego es más original, plantea problemas técnicos más ricos (simulación determinista, rendimiento en canvas, generación procedural, audio sintetizado, validación anti-trampas) y reutiliza entera la base SaaS del sprint. Ver [requisitos.md](requisitos.md#alcance-y-cambio-de-enfoque).

**Estado:** vigente.

## D-16 — Añadir el PBI P19 (despliegue en Google Cloud Run) a mitad del Sprint 1

**Qué se decidió:** Añadir el PBI P19 (despliegue en Google Cloud Run) a mitad del Sprint 1.

**Alternativas:** Seguir solo con Render.

**Por qué:** El profesor repartió créditos de Google Cloud para educación. Se documentó como cambio de alcance (tareas T29 y T30) y Render quedó como despliegue alternativo.

**Estado:** vigente.

## D-17 — Una rama y un pull request por tarea en el Sprint 1; un pull request por fase en el Sprint 2

**Qué se decidió:** Una rama y un pull request por tarea en el Sprint 1; un pull request por fase en el Sprint 2.

**Alternativas:** Commits directos a `main`; un PR por tarea también en el juego.

**Por qué:** En el Sprint 1 cada tarea de DoneTonic tiene rama `hito-N/Tx-…` y PR `[Tx] …` hacia la rama del hito, para una trazabilidad exacta. El juego se desarrolló por fases muy acopladas, así que cada fase es un PR a `main` y cada tarea de DoneTonic indica en su descripción el PR donde está. Ver [procesos.md](procesos.md#github-flow).

**Estado:** vigente.

## D-18 — Todos los PBIs con prioridad MUST

**Qué se decidió:** Todos los PBIs con prioridad MUST.

**Alternativas:** MoSCoW con COULD/SHOULD.

**Por qué:** Los del Sprint 1 son requisitos obligatorios del enunciado y los del Sprint 2 son la lógica del juego que pidió el profesor para ese sprint.

**Estado:** vigente.

## D-19 — Registrar el backlog del Sprint 2 en DoneTonic al cerrar el sprint

**Qué se decidió:** Registrar el backlog del Sprint 2 en DoneTonic al cerrar el sprint.

**Alternativas:** Registrarlo antes de programar.

**Por qué:** El juego se programó del 23 al 26/10, antes de que el profesor fijara que el Sprint 2 sería la lógica del juego. Por eso DoneTonic marca todos sus PBIs como hechos el 29/10 y la evolución real se reconstruye con las fechas de los PRs ([sprint-2.md](sprint-2.md#evolución-del-sprint-2)).

**Estado:** vigente.

## D-20 — Muñecos chibi de 16×20 píxeles con piernas

**Qué se decidió:** Muñecos chibi de 16×20 píxeles con piernas.

**Alternativas:** Sprites dibujados a mano.

**Por qué:** Primera versión del pixel art: muñecos generados por código en una cuadrícula pequeña.

**Estado:** sustituida por D-21.

## D-21 — Muñecos chibi de 32×32 sin piernas (cabeza grande, cuerpo y zapatones)

**Qué se decidió:** Muñecos chibi de 32×32 sin piernas (cabeza grande, cuerpo y zapatones).

**Alternativas:** Muñecos de 32×40 con piernas.

**Por qué:** Con más resolución cada jugador, entrenador y jefe es reconocible (peinado, barba, complexión, gesto), y quitar las piernas recupera el estilo chibi. Ver [log.md](log.md#2026-11-02--fase-7-muñecos-chibi-mapa-nuevo-menú-y-habilidades).

**Estado:** vigente.

## D-22 — Tope de bajas de una partida calculado con el ritmo real de aparición de rivales (`bajasPosibles` en `config.js`)

**Qué se decidió:** Tope de bajas de una partida calculado con el ritmo real de aparición de rivales (`bajasPosibles` en `config.js`).

**Alternativas:** Tope fijo de 20 rivales por segundo.

**Por qué:** Pasado el minuto 20 salen hasta 50 rivales por segundo y el tope fijo rechazaba como «no creíble» las partidas largas, que no se guardaban ni entraban en el ranking.

**Estado:** vigente.

## D-23 — Varias partidas en curso por cuenta (hasta 3) y reintentos al guardar

**Qué se decidió:** Varias partidas en curso por cuenta (hasta 3) y reintentos al guardar.

**Alternativas:** Una sola partida en curso.

**Por qué:** Con una sola, empezar otra (otra pestaña, «Empezar de nuevo») tiraba la anterior. El servidor rechaza las que se solapan en el tiempo para no cobrar dos partidas jugadas a la vez, y el cliente reintenta los fallos de red y avisa si no pudo guardar.

**Estado:** vigente.
