---
type: Diseño
title: Diseño del juego
description: Concepto, manual de usuario, sistemas, logros y evolución por fases del juego survivor rojiblanco.
timestamp: 2026-11-19
---

# El juego

La arquitectura del juego (módulos, servidor y anti-trampas) está en [arquitectura.md](arquitectura.md#arquitectura-del-juego); aquí se describe qué es y cómo funciona.

### Concepto

Un *survivor* (género popularizado por *Vampire Survivors*) con estética de píxeles «chibi»: controlas a un jugador de la historia del Atlético de Madrid, las armas atacan solas y tu trabajo es moverte, esquivar y elegir mejoras para aguantar cada vez más contra oleadas infinitas de rivales y jefes. La progresión entre partidas (club, fichajes, entrenamiento, logros) se guarda en el servidor.

### Cómo se juega (manual de usuario)

1. **Entrar:** con sesión iniciada, `#/jugar` abre el menú del juego. La primera vez pide un **mote** (3 a 16 caracteres, único) y una **cara** para el avatar.
2. **Menú:** Jugar, Vestuario (elegir jugador y equipación), Entrenamiento, Contratar, Logros, Combinaciones, Dirección (entrenador), Ranking y Ajustes; a la izquierda, tu ficha y tus tiempos.
3. **Controles:** WASD o flechas (en el móvil, joystick al arrastrar); `Espacio`/`E` ulti; `Esc`/`P` pausa; `F` pantalla completa; `1`–`3` para elegir mejora.
4. **Partida:** oleada nueva cada 30 s y un jefe cada cuatro oleadas (cada 2 minutos, el primero a los 1:30). Los rivales sueltan experiencia; al subir de nivel eliges una de tres mejoras (las que completan una combinación salen marcadas con ★).
5. **Ambientes:** cada 5 minutos cambia la pantalla: pretemporada → Final de la Copa del Rey → Final de Liga → Final de la Champions → Final del Mundial (todos con la camiseta de su selección). Los rivales salen más fuertes y suena la fanfarria de la competición.
6. **Fin:** al caer, pantalla de estadísticas (tiempo, ambiente, bajas, jefes, daño por habilidad, golpe más fuerte, daño recibido, logros nuevos) y recompensas del club.

### Sistemas

| Sistema | Descripción | Configuración |
|---|---|---|
| **Jugadores** | 36 (4 porteros, 9 defensas, 13 medios, 10 delanteros). La posición fija las estadísticas base; cada jugador tiene rasgos propios y una lista de habilidades que puede aprender | `PERSONAJES`, `POSICIONES` |
| **Rarezas** | Según lo que fue cada uno **en el Atleti** (años, títulos, peso en la historia): Leyenda (8), Épico (8), Raro (8) y Común (12). Más rareza = más habilidades (de 8 a 16) y menos probabilidad en un contrato | `RAREZAS` |
| **Entrenadores** | Cholo Simeone (+daño, +vida; ulti «¡Partido a partido!») y Luis Aragonés (+velocidad, +experiencia; ulti «Furia Roja») | `ENTRENADORES` |
| **Armas y habilidades** | 24, con 5 niveles: de disparo (balón, cañonazo de falta, córner, efecto, vaselina, chilena, volea, penalti), cuerpo a cuerpo (bufanda, banderines, cántico, silbato, tarjeta roja, VAR, cabezazo, entrada rasante, barrera, despeje), movilidad (regate, caño, pared, contragolpe) y especiales (himno de la Champions, bengala). Las más fuertes no suben hasta cierto minuto (`desdeMinuto`) y tienen recarga mínima (`cdMin`) | `ARMAS` |
| **Pasivas** | 14 (botas, imán, pasión, pizarra, cantera, masajista, espinilleras, pulmones, toque dorado, botas en llamas, paradón, brazalete, trébol, prórroga) | `PASIVAS` |
| **Combinaciones** | 24 «metas» que se activan al juntar ciertas habilidades a cierto nivel y dan bonus (Muro rojiblanco, Tiki-taka, Pichichi, Catenaccio, Leyenda rojiblanca, Golazo de otro planeta, Remontada épica, Triple Corona, Infierno del Metropolitano, Capitán eterno…) | `SINERGIAS` |
| **Ultras** | 6 que se consiguen solas al completar varias combinaciones: Premio Puskás, Ganador del Balón de Oro, MVP de la Champions, Trofeo Zamora, Bota de Oro histórica y Mejor jugador del Mundial. Efecto visual propio y daño en parte porcentual (menor contra jefes) | `ULTRAS`, `ultras.js` |
| **Rivales** | 16 equipos con jugadores genéricos; unos embisten (Liverpool, Arsenal, PSG, Dortmund) y otros disparan (Bayern, City, Chelsea, Juventus, Milan, Inter, Nápoles); élites blindado, veloz y furioso | `EQUIPOS`, `ELITES` |
| **Jefes** | 10: Guardiola (rondo, tiki-taka), Mourinho (autobús), Cristiano («Siuuu»), Messi (eslalon), Vinicius (zigzag), Yamal (rosca), Neymar (lluvia de bicicletas), Bale (cañonazos), Benzema (precisión) y Suárez (mordiscos). Tres fases por vida (66 % y 33 %), escolta de élites desde el segundo jefe, furia si tardas en ganarles y una jugada estrella en la fase 3. Desde la oleada 24 pueden salir dos a la vez y cada vuelta completa los hace un 25 % más duros | `JEFES`, `jefes.js` |
| **Dificultad** | Crecimiento continuo de vida, daño y velocidad; tramo final más duro desde el minuto 9 y rivales «curtidos» desde el 12; multiplicador por ambiente; ayuda inicial que se apaga en ocho minutos | `ESCALADO`, `TRAMO_FINAL`, `CURTIDOS`, `AMBIENTES` |
| **Movimiento** | Inercia, *ritmo* al correr (más daño y experiencia), castigo por quedarse quieto, premios por el campo | `FISICA`, `JUGADOR` |
| **Mundo** | Procedural e infinito por semilla, con ruidos suaves: distritos (ciudad, entrenamiento, parque, prado), estadios con gradas, plazas y tiendas, mesetas con escaleras, barro, lluvia y caminos. Colisión por los pies (vista 3/4) | `TERRENO`, `terreno.js` |
| **Club** | Plantilla inicial de 4 comunes (un jugador por posición); una ficha de contrato por cada 3 jefes de la cuenta; puntos de entrenamiento por jefes (1 cada 3) y por bajas; estadísticas Vida, Defensa, Disparo y Suerte (máx. 20) | `club.js` |
| **Logros** | 29 con un dato curioso de la historia del Atleti (tabla 9.6); 10 equipaciones desbloqueables | `logros.js` |
| **Sonido** | Efectos, música chiptune original (canción por ambiente que se anima con el tiempo y con los jefes, fanfarria por competición) y la grada (murmullo y cánticos según la distancia a las gradas) | `audio.js`, `musica.js` |

### Logros

| Logro | Cómo se consigue | Equipación |
|---|---|---|
| Primera muesca | Vence a tu primer jefe | Segunda |
| Camino de la final | Llega a la Final de la Copa del Rey (minuto 5) | |
| Campeones de Liga | Llega a la Final de Liga (minuto 10) | Tercera eléctrica |
| Noche europea | Llega a la Final de la Champions (minuto 15) | Noche europea |
| Final del Mundial | Llega a la Final del Mundial (minuto 20) | Final del Mundial |
| Tercero en París | Consigue la ultra «Ganador del Balón de Oro» con Griezmann | |
| Bota de Oro charrúa | Consigue la ultra «Bota de Oro histórica» con Forlán | |
| Hamburgo, 2010 | Llega a la Final de la Champions con Forlán o con el Kun | |
| Noche de Mónaco | Vence a 3 jefes en una partida con Falcao | |
| El cabezazo del Camp Nou | Consigue la combinación «Dominio aéreo» con Godín | |
| Brazalete eterno | Consigue la combinación «Capitán eterno» con Gabi | |
| El Niño | Llega a la Final de la Copa del Rey con Fernando Torres | |
| Del Oporto al Calderón | Consigue la combinación «Jugón» con Futre | |
| Doblete del 96 | Llega a la Final de Liga con Simeone o con Pantić | Doblete del 96 |
| Bernabéu, 2013 | Llega a la Final de la Copa del Rey con Miranda o con Courtois | |
| Cinco Zamoras | Consigue la ultra «Trofeo Zamora» con Oblak | Leyenda bajo palos |
| Los 27 de Costa | Elimina a 270 rivales en una partida con Diego Costa | |
| La Araña campeona | Llega a la Final del Mundial con Julián Álvarez | |
| El récord de Adelardo | Elimina a 553 rivales en una sola partida | |
| El Sabio de Hortaleza | Aguanta 10 minutos con Luis Aragonés en el banquillo | |
| Partido a partido | Aguanta 15 minutos con el Cholo en el banquillo | |
| El muro | Aguanta los 5 primeros minutos sin que te toquen | |
| Poder de leyenda | Consigue tu primera ULTRA | |
| Leyenda total | Consigue tres ULTRAS en una partida | |
| Mil en el Metropolitano | Elimina a 1000 rivales en una partida | |
| El Pupas | Que te eliminen antes del minuto 2 | El Pupas |
| Toda una vida | Juega 20 partidas con Koke | Adiós al Calderón |
| Azul y blanco | Ten 20 jugadores en tu plantilla | Centenario 1903 |
| Paseo de Leyendas | Ficha a las 8 leyendas | |

Cada logro esconde un dato real del club (por ejemplo, que Griezmann fue tercero en el Balón de Oro de 2018, que Oblak ganó cinco Zamoras seguidos o que el Atleti nació en 1903 vestido de azul y blanco). Los datos están en `logros.js` y se han comprobado uno a uno.

### Evolución por fases

Cada fase es una etiqueta del Sprint 2 en DoneTonic ([sprint-1.md](sprint-1.md) y [sprint-2.md](sprint-2.md)).

| Fase | Qué se añadió | Problemas que resolvió |
|---|---|---|
| Rediseño | Nueva interfaz (tokens, tipografía, modo oscuro rojiblanco), menú del juego como inicio con sesión | La interfaz del sprint era funcional pero sobria |
| 1 | Survivor base: simulación, oleadas, armas, mejoras, jefes y menú con el muñeco paseando | — |
| 2 | Club en el servidor (plantilla, contratos, entrenamiento), ranking de la grada, élites y poderes nuevos | Progreso ligado a la cuenta y validado |
| 3 | 6 ultras, 24 combinaciones, dificultad con castigo, entrenamiento por bajas y modo pruebas | Maxear una sola cosa ya no basta |
| 4 | Mundo procedural natural, 5 ambientes por finales, mote y avatar | Mapa repetitivo; correo visible en el ranking |
| 5 | Rarezas por historia rojiblanca, habilidades niveladas, combinaciones a la vista, animación de la firma, colisiones exactas | Regate demasiado fuerte; esconderse detrás de los tejados |
| 6 | 29 logros con historia y 10 equipaciones, estadísticas de fin, música chiptune y grada, muñecos con más detalle, jefes con escolta, furia y jugada estrella | Falta de objetivos a largo plazo; sonido pobre; jefes fáciles |
| 7 | Muñecos chibi de 32×32 (sin piernas) con rasgos propios, mapa con mucho más detalle y luz por ambiente, menú rediseñado (paneles translúcidos, centro despejado y botón de entrada), 5 habilidades (carambola, presión alta, la ola, fuera de juego y ruleta), 2 pasivas (visión de juego y partidos grandes), 5 combinaciones y mejores efectos de todas las habilidades | Muñecos casi iguales entre sí; mapa plano; menú estático; efectos poco legibles |
