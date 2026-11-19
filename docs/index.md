# Bundle documental de La Grada Colchonera

Este bundle documenta el estado comprobable de La Grada Colchonera: un juego web rojiblanco tipo *survivor* en pixel art construido sobre una aplicación SaaS en capas (Node.js, Express, PostgreSQL y Google Cloud Run). El **Sprint 1** (Hitos 1–4, la base SaaS) y el **Sprint 2** (la lógica del juego) están cerrados; después se añadió la Fase 7 del juego y varios arreglos. El tablero de trabajo está en [DoneTonic](https://app.donetonic.com) (espacio *TFG*, equipo *Colchoneros*). Para el contexto general del producto y la ejecución local, consulta el [README](../README.md).

- [log.md](log.md): cronología de pull requests y actualizaciones, de la más reciente a la más antigua.
- [arquitectura.md](arquitectura.md): capas, modelo de datos, API, flujos con diagramas de secuencia, seguridad, arquitectura del juego y manejo de errores.
- [requisitos.md](requisitos.md): contexto, alcance y estado de cada requisito funcional y no funcional.
- [decisiones.md](decisiones.md): decisiones técnicas, de proceso y de producto, con alternativas y estado.
- [procesos.md](procesos.md): Scrum con DoneTonic, definición de hecho, GitHub Flow, pruebas, CI/CD y configuración de servicios externos.
- [sprint-1.md](sprint-1.md): hitos, PRs y etiquetas, backlog, trazabilidad, revisión frente al enunciado y retrospectiva del primer sprint.
- [sprint-2.md](sprint-2.md): backlog, historias de usuario, trazabilidad, evolución, trabajo posterior, métricas y retrospectiva del segundo sprint.
- [juego.md](juego.md): concepto, manual de usuario, sistemas, logros y evolución por fases del juego.
- [glosario.md](glosario.md): términos que aparecen en la documentación.
- [capturas/](capturas/README.md): capturas de la aplicación, del juego, de producción y de DoneTonic, con lo que muestra cada una.

## Uso para la memoria

La memoria está en [`memoria/`](../memoria/README.md) (LuaLaTeX, estilo TFG). Cada capítulo se alimenta de estos documentos:

| Capítulo de la memoria | Fichero | Documentos |
|---|---|---|
| Resumen / abstract | `preliminares/resumen.tex` | [requisitos.md](requisitos.md) (contexto) y [sprint-2.md](sprint-2.md) (métricas y conclusiones) |
| 1. Introducción | `capitulos/01-introduccion.tex` | [requisitos.md](requisitos.md) |
| 2. Gestión del proyecto | `capitulos/02-gestion.tex` | [procesos.md](procesos.md), [sprint-1.md](sprint-1.md) y [sprint-2.md](sprint-2.md) |
| 3. Arquitectura | `capitulos/03-arquitectura.tex` | [arquitectura.md](arquitectura.md) y [decisiones.md](decisiones.md) |
| 4. Implementación | `capitulos/04-implementacion.tex` | [arquitectura.md](arquitectura.md) y [juego.md](juego.md) |
| 5. Pruebas, errores y logs | `capitulos/05-pruebas.tex` | [procesos.md](procesos.md#pruebas) y [arquitectura.md](arquitectura.md#registro-de-actividad-y-manejo-de-errores) |
| 6. CI/CD | `capitulos/06-cicd.tex` | [procesos.md](procesos.md#integración-y-despliegue-continuos) |
| 7. Conclusiones | `capitulos/07-conclusiones.tex` | retrospectivas de [sprint-1.md](sprint-1.md#retrospectiva) y [sprint-2.md](sprint-2.md#retrospectiva) |
| Anexo A. Manual de usuario | `anexos/A-manual-usuario.tex` | [juego.md](juego.md#cómo-se-juega-manual-de-usuario) y [capturas](capturas/README.md) |
| Anexo B. Trazabilidad | `anexos/B-trazabilidad.tex` | [requisitos.md](requisitos.md) y las tablas de trazabilidad de los sprints |

Los diagramas están en Mermaid (se exportan con `npx @mermaid-js/mermaid-cli -i diagrama.mmd -o diagrama.pdf`) y `\graphicspath` de `memoria/main.tex` ya busca en todas las carpetas de `capturas/`.
