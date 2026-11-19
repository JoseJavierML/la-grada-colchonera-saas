---
type: Proceso
title: Proceso: Scrum, GitHub Flow, pruebas, CI/CD y configuración
description: Cómo se ha trabajado: Scrum con DoneTonic, definición de hecho, flujo de ramas y pull requests, estrategia de pruebas, integración y despliegue continuos y configuración de los servicios externos.
timestamp: 2026-11-19
---

# Procesos

## Scrum con DoneTonic

- **Herramienta:** DoneTonic, espacio de trabajo *TFG*, equipo *Colchoneros*.
- **Sprint 1:** del 29/09/2026 al 27/10/2026. 19 PBIs (P1–P19) con 32 tareas (T1–T32), etiquetados por hito (*Hito 1* a *Hito 4*). Todos son **MUST** porque son requisitos obligatorios del enunciado.
- **Sprint 2:** del 23/10/2026 al 29/10/2026. 29 PBIs (P20–P48) con 50 tareas (T33–T82), etiquetados por fase del juego (*Base del juego*, *Fase 2 · Club*, *Fase 3 · Ultras*, *Fase 4 · Mundo*, *Fase 5 · Rarezas*, *Fase 6 · Logros y sonido* y *Calidad*). También son **MUST**: el profesor fijó la lógica del juego como objetivo del sprint.
- **Historias de usuario:** cada PBI tiene la suya («Como aficionado quiero … para …») y sus criterios de aceptación.
- **Del enunciado al backlog:** cada requisito se convirtió en un PBI con descripción y criterios de aceptación; cada PBI se dividió en una o dos tareas; cada tarea es una rama y un PR.
- **Seguimiento:** tablero por estados, Sprint Burnup y Burndown, Backlog Burndown y Burnup y Velocity Chart (capturas en `docs/capturas/donetonic/`, tomadas el 29/10/2026 con los dos sprints cerrados).
- **Registro del Sprint 2:** el juego se programó del 23 al 26/10, antes de que el profesor fijara el contenido del Sprint 2, y su backlog se registró en DoneTonic al cerrar el sprint (29/10). Por eso DoneTonic marca todos sus PBIs como hechos ese día; la evolución real se reconstruye con las fechas de los PRs ([sprint-2.md](sprint-2.md#evolución-del-sprint-2)).
- **Cambio de alcance durante el sprint:** el profesor repartió créditos de Google Cloud para educación y se añadió el PBI **P19** (despliegue en Google Cloud Run, tareas T29 y T30). Render se mantiene como despliegue alternativo.

1. **Del enunciado al backlog.** Cada requisito se convirtió en un PBI con una historia de usuario («Como … quiero … para …») y criterios de aceptación. En el Sprint 1 los requisitos venían del enunciado; en el Sprint 2, de la lógica del juego, agrupada por fases.
2. **Del PBI a las tareas.** Cada PBI se dividió en el menor número de tareas que tuviera sentido (entre una y tres).
3. **De la tarea al código.** En el Sprint 1 cada tarea tiene una rama propia `hito-N/Tx-descripcion` que se integra en la rama del hito mediante un PR `[Tx] ...`. En el Sprint 2 el trabajo se integró en `main` con **un PR por fase** del juego (#36, #44–#54), y cada tarea de DoneTonic indica en su descripción el PR donde está (`PR: #nn`).
4. **Integración y despliegue.** Todos los PRs exigen el CI en verde y, al llegar a `main`, la aplicación se despliega sola en Google Cloud Run.
5. **Seguimiento.** Al fusionar cada PR, su tarea pasa a *HECHO* en el tablero. Cuando todas las tareas de un PBI están hechas, el PBI pasa a *HECHO* automáticamente. Las gráficas de DoneTonic se actualizan solas.

## Definición de hecho (DoD)

Una tarea solo se considera **hecha** cuando cumple todo esto:

- el código está en un PR con la referencia a la tarea (en el título en el Sprint 1, en la descripción de la tarea en el Sprint 2);
- las pruebas automáticas pasan en el CI (check **Pruebas**), y las partes nuevas tienen sus propias pruebas;
- el PR está fusionado y desplegado en la URL pública (https://la-grada-colchonera-1081564062572.europe-southwest1.run.app);
- la documentación (README, `docs/`, `.env.example`) está actualizada si hace falta.

Un **hito** (Sprint 1) está hecho cuando su rama está en `main`, desplegada y publicada como release. Una **fase del juego** (Sprint 2) está hecha cuando su PR está en `main`, desplegado y probado a mano en el navegador.

## GitHub Flow

```mermaid
flowchart TB
    T["Tarea Tx en DoneTonic"] --> B["Rama hito-N/Tx-descripcion<br/>desde hito-N/integracion"]
    B --> C["Commits [Tx] ... + npm test en local"]
    C --> PR["PR [Tx] → hito-N/integracion"]
    PR --> CI{"CI: Pruebas + imagen Docker"}
    CI -- "rojo" --> C
    CI -- "verde" --> M["Fusión"]
    M --> D["Tarea HECHA en el tablero"]
    M --> H{"¿Todas las tareas del hito?"}
    H -- "sí" --> PRH["PR hito-N/integracion → main"]
    PRH --> CI2{"CI"}
    CI2 -- "verde" --> MAIN["main → despliegue automático"]
    MAIN --> REL["Release hito-N"]
```

- `main` y `hito-*/integracion` están **protegidas**: solo admiten cambios por PR y exigen el CI en verde.
- Commits en español, en imperativo y con el identificador de la tarea (`[T5] Añade el repositorio de usuarios en memoria`), según [`CONTRIBUTING.md`](../CONTRIBUTING.md).
- **Hito 4:** una rama por tarea (`hito-4/T22-google-oauth`, `T23-correo`, `T24-confirmacion`, `T25-secretos-hito4`, `T26-privacidad`) integrada directamente en `main` con su PR (#37–#41). Como dependían unas de otras, se reubicaron (rebase) sobre el `main` real antes de abrir los PRs.
- **Sprint 2 (juego):** ramas `feature/rediseno-y-juego`, `juego/personajes-pixel` y `juego/movilidad-hub`, con un PR a `main` por fase (#36 y #44–#54) y uno por arreglo (#51, #55, #56). Cada tarea de DoneTonic indica en su descripción el PR donde está.

## Pruebas

### Estrategia

- **Framework:** `node:test` (incluido en Node, sin dependencias) y `node:assert/strict`.
- **Un único comando:** `npm test`. En local no hace falta base de datos (`pg-mem`); en el CI se ejecutan contra un PostgreSQL 17 real.
- **Niveles:**
  - **Unitarias** de la lógica (`userService`, `tokenSigner`, `domainPolicy`, `logger`, `mailer`) con dependencias falsas inyectadas.
  - **Contrato del repositorio:** la misma batería contra la implementación en memoria y la de PostgreSQL.
  - **Integración de la API:** peticiones HTTP reales a la app montada en un puerto aleatorio (`tests/helpers.js`), con cookies, cubriendo `401`, `403`, `409`…
  - **Integración con OAuth y correo:** Google y SMTP sustituidos por versiones falsas (no se llama a nadie).
  - **Base de datos real** (`api.database.test.js`, solo en el CI): registro, inicio de sesión y sesión guardada en PostgreSQL.
  - **Frontend estático:** que solo `api.js` use `fetch`, que no se meta texto de usuario como HTML y que todos los módulos se importen.
  - **Simulación del juego:** mundos con semilla avanzados paso a paso (jefes, ultras, colisiones, alcance del mapa, equilibrio, logros).
- **Casos de error primero:** muchas pruebas comprueban rechazos (email repetido, cuenta pendiente, usuario normal que intenta eliminar a otro, último administrador, enlace manipulado o caducado, `state` reutilizado, resultado de partida imposible, dos contratos a la vez).
- **Reloj y azar inyectables** para probar la caducidad de los enlaces y la validación de las partidas sin esperar.

### Pruebas por fichero

| Fichero | Pruebas | Qué comprueba |
|---|---:|---|
| `userService.test.js` | 27 | Registro, email repetido o inválido, contraseña corta, autenticación, pendiente, eliminada, volver con el mismo email tras la baja, OAuth, roles, bajas, último administrador, primer administrador |
| `api.auth.test.js` | 16 | Registro (201 sin hash), 409/400, login, mismo mensaje en 401, cookie `HttpOnly`, `me`, logout, sesión de una cuenta eliminada, confirmación, enlace no válido, URL pública, reenvío 202, correo caído (503) |
| `userRepository.test.js` | 16 | Contrato del repositorio en memoria y en PostgreSQL, incluida la cuenta eliminada que no ocupa su email |
| `frontend.test.js` | 14 | Separación interfaz/cliente, sin HTML de usuario, módulos importables |
| `domainPolicy.test.js` | 13 | Prueba técnica (8 de rechazo o error) |
| `api.users.test.js` | 11 | 401, 403, listado, activo, cambio de rol, bajas propias y ajenas, último administrador |
| `googleOAuth.test.js` | 11 | Proveedores disponibles, redirección con `state`, cuenta nueva, existente y pendiente que se confirma, `state` incorrecto, ausente y reutilizado, cancelación, email sin verificar, volver a entrar con Google tras darse de baja (cuenta nueva), cliente real con `fetch` falso y configuración |
| `config.test.js` | 8 | Secretos obligatorios en producción, valores por defecto, `.env` fuera del repositorio |
| `accountConfirmation.test.js` | 6 | Correo con enlace, confirmación, enlace manipulado/caducado, eliminada, doble confirmación, reenvío |
| `logger.test.js` | 6 | JSON, niveles, ocultación de datos sensibles |
| `tokenSigner.test.js` | 3 | Firma, manipulación y caducidad |
| `mailer.test.js` | 3 | SMTP o registro |
| `migrate.test.js` | 3 | Crea las tablas, es idempotente y aplica las restricciones (email único, roles válidos) |
| `bootstrap.test.js` | 2 | Arranque con memoria y con PostgreSQL |
| `health.test.js` | 1 | `/api/health` |
| `api.database.test.js` | CI | Recorrido completo con PostgreSQL real |
| `juego-club.test.js` | 27 | Club, contratos, entrenamiento, rarezas, ranking |
| `juego-movilidad.test.js` | 22 | Mundo, colisiones, mesetas, alcance del mapa, ambientes |
| `api.juego.test.js` | 15 | API del juego, anti-trampas, logros en el servidor, concurrencia, cuenta eliminada que sale del ranking |
| `juego-personajes.test.js` | 14 | Personajes, entrenadores, habilidades por jugador |
| `juego-habilidades.test.js` | 14 | Habilidades nuevas y niveles por minuto |
| `juego-ultras.test.js` | 13 | Ultras: cómo se consiguen y qué hacen |
| `juego-logros.test.js` | 13 | Logros, equipaciones, estadísticas, pantalla de fin, música y jefes más duros |
| `game.test.js` | 10 | Simulación base (paso fijo, semilla, armas, jefes, ulti, dificultad) |
| `juegoRepository.test.js` | 8 | Repositorio del juego en memoria y PostgreSQL |
| `juego-sprites.test.js` | 9 | Fase 7: muñecos chibi de 32×32 sin piernas, una cabeza distinta por persona, peinados y barbas, fotogramas, rivales variados y caché |
| `juego-habilidades-nuevas.test.js` | 8 | Fase 7: carambola, presión alta, la ola, fuera de juego, ruleta, pasivas y combinaciones nuevas |
| `juego-mapa.test.js` | 5 | Fase 7: sombras coherentes, edificios en su rectángulo de colisión, público animado, texturas repartidas entre fotogramas |
| `juego-menu.test.js` | 7 | Fase 7: menú animado, tarjetas y consejos del míster, movimiento reducido, pausa con árbol de habilidades y salida con confirmación |
| **Total** | **322** | 140 del Sprint 1 (cuentas, API, datos, seguridad, prueba técnica), 136 del juego hasta la Fase 6 y 46 de la Fase 7 (incluidos los arreglos de guardado, hitboxes y rendimiento) |

### Comprobaciones manuales y visuales

Además de las pruebas automáticas, cada cambio visible se ha comprobado en el navegador con Playwright (capturas de pantalla automatizadas de cada pantalla y estado), y el Hito 4 se probó a mano en producción (correo real recibido en Gmail, enlace manipulado, acceso con dos cuentas de Google distintas). Las capturas de la [capturas](capturas/README.md) se generan con la aplicación real y datos de ejemplo.

## Integración y despliegue continuos

| Workflow | Cuándo | Qué hace |
|---|---|---|
| `ci.yml` (*CI*) | Cada PR y cada push a `main` y a `hito-*/integracion` | Trabajo **Pruebas**: `npm ci` + `npm test` con un servicio PostgreSQL 17. Trabajo **Imagen Docker**: construye la imagen, la arranca y comprueba `/api/health` |
| `deploy-cloud-run.yml` | Cuando el CI termina bien en `main` (`workflow_run`) | Autentica en Google Cloud con Workload Identity Federation, construye y sube la imagen a Artifact Registry y despliega en Cloud Run con las variables (`NODE_ENV`, `ALLOWED_DOMAINS`) y los 8 secretos de Secret Manager |
| `memoria.yml` | Cambios en `memoria/` | Compila la memoria con LuaLaTeX (latexmk + biber) y publica el PDF como artefacto |
| Render | Cada cambio en `main` | Despliegue alternativo según `render.yaml` |

- **Imagen Docker:** Node 22 (Alpine), solo dependencias de producción, usuario sin privilegios, escucha en el 8080.
- **Cloud Run:** región `europe-southwest1` (Madrid), escala a cero, secretos montados como variables de entorno, cuenta de servicio de ejecución con acceso solo a sus secretos.
- **Base de datos:** Neon (PostgreSQL gestionado, región Frankfurt), compartida por Cloud Run y Render.
- **Seguridad del despliegue:** GitHub no guarda ninguna clave de Google Cloud; obtiene un token temporal por federación de identidad.
- **Resiliencia:** si una revisión no arranca (por ejemplo, falta un secreto), Cloud Run y Render siguen sirviendo la anterior.
- **Preparación única:** `scripts/gcp/preparar-cloud-run.sh` (proyecto, APIs, Artifact Registry, secretos, cuentas de servicio, WIF) y `scripts/gcp/configurar-hito4.sh` (secretos de correo y Google). Pasos en [configuración de los servicios externos](#configuración-de-los-servicios-externos-google-y-correo).

## Configuración de los servicios externos (Google y correo)

Esta guía deja listos los dos servicios externos del Hito 4. Se hace una sola vez.

URL de la aplicación: `https://la-grada-colchonera-1081564062572.europe-southwest1.run.app`

### Cliente OAuth de Google (inicio de sesión)

Se hace en el mismo proyecto de Google Cloud donde está Cloud Run.

1. Entra en <https://console.cloud.google.com/auth/overview> y elige el proyecto `la-grada-colchonera-…`.
2. Si te pide configurar la pantalla de consentimiento (*Google Auth Platform → Comenzar*):
   - Nombre de la aplicación: `La Grada Colchonera`. Correo de asistencia: el tuyo.
   - Público: **Externo**. Correo de contacto: el tuyo. Acepta las condiciones y crea.
3. En **Público** (*Audience*) deja la aplicación en *Pruebas* y añade tu correo (y el del profesor, si va a probarlo) como **usuarios de prueba**. Para que entre cualquiera, pulsa *Publicar aplicación*: con los permisos básicos (`openid`, `email`, `profile`) no hace falta verificación.
4. En **Clientes → Crear cliente**:
   - Tipo de aplicación: **Aplicación web**. Nombre: `La Grada Colchonera web`.
   - **URIs de redireccionamiento autorizados** (añade las dos):
     - `https://la-grada-colchonera-1081564062572.europe-southwest1.run.app/api/auth/google/callback`
     - `http://localhost:3000/api/auth/google/callback`
5. Al crear te muestra el **ID de cliente** y el **secreto del cliente**. Cópialos: son `GOOGLE_CLIENT_ID` y `GOOGLE_CLIENT_SECRET`.

### Correo saliente (SMTP con Gmail)

1. En la cuenta de Gmail que enviará los correos, activa la **verificación en dos pasos** (<https://myaccount.google.com/security>).
2. Entra en <https://myaccount.google.com/apppasswords>, crea una contraseña de aplicación llamada `La Grada` y copia las 16 letras (sin espacios).
3. La `SMTP_URL` queda así (la `@` del usuario se escribe `%40`):

   ```
   smtps://tucorreo%40gmail.com:abcdefghijklmnop@smtp.gmail.com:465
   ```

   y `MAIL_FROM`: `La Grada Colchonera <tucorreo@gmail.com>`.

> Gmail limita los envíos diarios (unos 500), de sobra para una práctica. Si prefieres no usar tu cuenta personal, Brevo ofrece SMTP gratuito con el mismo formato de URL.

### Probar en local

Copia `.env.example` a `.env` y rellena `SMTP_URL`, `MAIL_FROM`, `GOOGLE_CLIENT_ID` y `GOOGLE_CLIENT_SECRET`, y arranca con `npm start`. Sin `SMTP_URL`, los correos no se envían: el enlace de confirmación aparece en la consola.

### Producción (Cloud Run)

1. En Cloud Shell, desde la carpeta `scripts/gcp`: `PROJECT_ID=tu-proyecto bash configurar-hito4.sh` (pide los cuatro valores y los guarda en Secret Manager).
2. Fusiona la PR del despliegue (T25): a partir de ahí Cloud Run recibe esos secretos como variables de entorno.
3. Comprueba en la web: registro → llega el correo → enlace → entrar, y el botón *Entrar con Google*.

### Qué ocurre con las cuentas

| Caso | Resultado |
|---|---|
| Registro con email y contraseña | Cuenta `pending` + correo con enlace (24 h). Hasta confirmar, el login devuelve `ACCOUNT_PENDING` |
| Entrar con Google, email nuevo | Cuenta creada ya activa (`authProvider: google`) |
| Entrar con Google, email de una cuenta local pendiente | La cuenta queda confirmada (Google ya verificó el correo) |
| Entrar con Google, email de una cuenta eliminada | Se crea una cuenta nueva: el email queda libre tras la baja (PR #56) |
| Administrador inicial | Se crea activo desde `ADMIN_EMAIL` / `ADMIN_PASSWORD` |

## Variables de entorno

Los nombres están en [`.env.example`](../.env.example) y explicados en el [README](../README.md#variables-de-entorno-y-secretos); los valores nunca se escriben en el repositorio. En producción vienen de Secret Manager (`database-url`, `session-secret`, `admin-email`, `admin-password`, `smtp-url`, `mail-from`, `google-client-id` y `google-client-secret`) y de las variables `NODE_ENV` y `ALLOWED_DOMAINS` del workflow de despliegue.
