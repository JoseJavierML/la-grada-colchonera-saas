# Cronología

## 2026-11-19 — Reorganización de `docs/` como bundle documental

Rama `docs/bundle`. La documentación de `docs/` se reorganiza como un bundle: un [index.md](index.md) de entrada, esta cronología y un documento por tema con su cabecera (`type`, `title`, `description` y `timestamp`): [arquitectura.md](arquitectura.md), [requisitos.md](requisitos.md), [decisiones.md](decisiones.md), [procesos.md](procesos.md), [sprint-1.md](sprint-1.md), [sprint-2.md](sprint-2.md), [juego.md](juego.md) y [glosario.md](glosario.md). El contenido sale de los documentos anteriores (`documentacion-tecnica.md`, `gestion/README.md`, `revision-entrega.md` y `hito4-configuracion.md`), que desaparecen; se elimina también `gestion/README-1.md`, una copia del README subida por error. Las decisiones pasan a tener identificador (D-01 a D-23), los requisitos una columna de estado y cada sprint su retrospectiva con los problemas encontrados. Las capturas no cambian de sitio y su índice pasa a [capturas/README.md](capturas/README.md). Se actualizan los enlaces del README y de la memoria. No cambia el código.

## 2026-11-03 — Guardado de partidas, experiencia, hitboxes y rendimiento

PR [#59](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/59), rama `arreglo/guardado-rendimiento`. Las partidas largas no se guardaban ni entraban en el ranking porque el servidor rechazaba más de 20 rivales eliminados por segundo, y pasado el minuto 20 salen hasta 50; el tope pasa a calcularse con el ritmo real de aparición (`bajasPosibles` en `config.js`, decisión [D-22](decisiones.md)). La experiencia dejaba de salir con el campo lleno de gemas; ahora la gema más lejana se trae a donde cae el rival. El cliente reintenta los fallos de red al guardar y avisa si una partida no se pudo guardar, el servidor admite hasta tres partidas en curso ([D-23](decisiones.md)) y la pantalla final muestra lo guardado. Hitboxes: balones contra la cabeza y el cuerpo de los muñecos, ataques de los jefes medidos desde los pies, monumento sin paredes invisibles y obstáculos que ya no se pintan con el dibujo de un mapa anterior. Rendimiento: no se dibuja lo que queda fuera de pantalla, la oscuridad de la noche se pinta a menor resolución, el lienzo tiene un tope de 1920×1080 píxeles reales y la búsqueda de vecinos no crea objetos en cada paso. La suite pasa a 322 pruebas.

## 2026-11-02 — Fase 7: muñecos chibi, mapa nuevo, menú y habilidades

PR [#58](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/58), rama `juego/fase-7`. Muñecos chibi de 32×32 sin piernas con rasgos propios por jugador, entrenador y jefe ([D-21](decisiones.md)); mapa con mucho más detalle, sombras coherentes, público animado y luz propia por ambiente; menú rediseñado con paneles translúcidos, centro despejado, título rojiblanco y botón de entrada de partido; pantallas del menú que caben enteras y se cierran con un clic fuera; pausa con ajustes, árbol de habilidades, combinaciones y ultras, y confirmación al salir. Se añaden 5 armas (gol de carambola, presión alta, la ola, fuera de juego y ruleta), 2 pasivas y 5 combinaciones, y mejores efectos en todas las habilidades. Se trabajó en cuatro ramas paralelas (muñecos, mapa, menú y habilidades) integradas con las pruebas en verde.

## 2026-10-29 — Gestión del Sprint 2 y arreglo de la baja con Google

PR [#57](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/57), rama `docs/sprint-2`: el backlog del Sprint 2 (29 PBIs y 50 tareas) se registra y se cierra en DoneTonic ([D-19](decisiones.md)), con historias de usuario, trazabilidad tarea → PR, capturas del tablero y el README completo con las capturas del juego. PR [#56](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/56), rama `arreglo/volver-tras-baja`: quien eliminaba su cuenta no podía volver con el mismo email ni con Google; la migración 005 hace el email único solo entre las cuentas no eliminadas, se suelta el mote y la cuenta sale del ranking. Cierre del Sprint 2.

## 2026-10-27 — README completo y documentación técnica

PR [#55](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/55), rama `docs/readme-final`. Capturas nuevas de los Hitos 1–4 con la interfaz rediseñada, README completo y documentación técnica para la memoria. Al hacer las capturas aparece un fallo: el primer clic en «Reenviar el correo» se perdía tras un error; se arregla en el mismo PR. Fin del Sprint 1 en DoneTonic.

## 2026-10-26 — Fases 5 y 6 del juego

PRs [#53](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/53) y [#54](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/54), rama `juego/movilidad-hub`. Fase 5: rarezas por historia rojiblanca y 36 jugadores, habilidades que no suben hasta cierto minuto, combinaciones y ultras a la vista, animación de la firma y colisiones exactas. Fase 6: 29 logros con un dato de la historia del Atleti y 10 equipaciones, estadísticas de fin de partida, música chiptune y la grada, muñecos con más detalle y jefes más duros.

## 2026-10-24 — Contenido del juego y Fases 2 a 4

PRs [#44](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/44)–[#52](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/52), ramas `juego/personajes-pixel` y `juego/movilidad-hub`. Personajes, entrenadores, jefes y 16 rivales en pixel art (#44); movilidad, barro, lluvia, gradas, rivales que disparan y jefes con fases (#45); cámara 3/4 (#46); mapa procedural con biomas (#47); 19 jugadores con habilidades propias, 12 armas, combinaciones y 6 jefes nuevos (#48). Fase 2: club en el servidor con contratos, entrenamiento y ranking (#49, migración 003). Fase 3: ultras, dificultad y modo pruebas (#50). Arreglo: el juego no cargaba por una variable usada antes de existir (#51). Fase 4: mundo procedural natural, cinco ambientes por finales, mote y avatar (#52, migración 004).

## 2026-10-23 — Hito 4 y comienzo del juego

PRs [#37](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/37)–[#41](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/41), ramas `hito-4/T22`–`T26`: secretos del Hito 4 en Cloud Run (#37), servicio de correo con enlaces firmados (#38), confirmación de la cuenta (#39), inicio de sesión con Google (#40) y política de privacidad (#41). Las ramas se desarrollaron en cadena y se reubicaron sobre `main` antes de abrir los PRs. PRs #42 y #43 documentan el cierre del Hito 4. Etiquetas `hito-4` y `sprint-1-final`. El mismo día empieza el Sprint 2 con el PR [#36](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/36), rama `feature/rediseno-y-juego`: rediseño completo del frontend y base del juego survivor ([D-15](decisiones.md)).

## 2026-10-15 — Prueba técnica (variante 3)

PRs [#34](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/34) y [#35](https://github.com/VelaElDios/la-grada-colchonera-saas/pull/35), ramas `PruebaTecnicaSprint1` y `docs/prueba-tecnica-variante3`. Solo se admiten registros de los dominios de correo de `ALLOWED_DOMAINS`, con la lógica en `domainPolicy.js`, el endpoint `POST /api/prueba/dominio` y 13 pruebas. Un JSON mal formado pasa a responder `400` en JSON en toda la API.

## 2026-10-12 — Primera entrega del Sprint 1 (Hitos 1–3)

Integración de los hitos en `main` con un PR por tarea hacia la rama de cada hito y un PR de cierre por hito. Hito 1: PRs #1–#8 y #10, cierre #9, etiqueta `hito-1`. Hito 2: PRs #11–#16, cierre #17, etiqueta `hito-2`. Hito 3: PRs #18–#23, cierre #24, etiqueta `hito-3`. Cierre del sprint: README, revisión de la entrega, Docker y despliegue en Cloud Run (PRs #25–#28, cierre #29, [D-16](decisiones.md)), revisión frente al enunciado, carpeta de la memoria y capturas de DoneTonic (PRs #30, #31 y #33, cierre #32). Etiqueta `sprint-1`.

## 2026-09-29 — Comienzo del Sprint 1

Se crea el repositorio y el tablero en DoneTonic (espacio *TFG*, equipo *Colchoneros*) con los 18 PBIs iniciales del enunciado, priorizados por hito y todos MUST ([D-18](decisiones.md)).
