# MOP-13 · Turno de noche en el espacio

> «Puedes salvar la galaxia, pero si no has fregado el comedor, no cobrarás las horas extra.»

**Trabajo Fin de Máster · Máster de Desarrollo con IA**

MOP-13 es un juego web cooperativo en pixel art isométrico. Eres el conserje del turno de noche de la nave
**USC Desastre**: la tripulación atiende las emergencias y tú friegas lo que deja. Se juega en el navegador,
sin instalar nada, solo o con hasta 3 amigos más y chat de voz.

| Recurso | Enlace |
|---|---|
| 🎮 **Juego desplegado** | https://khaki-elk-914724.hostingersite.com |
| 💻 **Código fuente** | https://github.com/nastxp/Mop13 |
| 🖥️ **Slides (HyperFrames)** | https://khaki-elk-914724.hostingersite.com/slides/ · [PDF en el repositorio](slides/MOP-13-presentacion.pdf) |
| 🎬 **Vídeo de presentación** | _Pendiente: añadir aquí la URL pública del vídeo_ |
| 🔑 **Usuario y contraseña de prueba** | No hacen falta: el juego no tiene login |

![Partida en curso: MOP-13 junto a la estación de limpieza, con la tripulación en el comedor, los dormitorios y los baños](slides/composition/img/play2.jpg)

---

## Índice

1. [Descripción general](#1-descripción-general)
2. [Funcionalidades principales](#2-funcionalidades-principales)
3. [Stack tecnológico](#3-stack-tecnológico)
4. [Arquitectura](#4-arquitectura)
5. [Instalación y ejecución](#5-instalación-y-ejecución)
6. [Estructura del proyecto](#6-estructura-del-proyecto)
7. [Despliegue](#7-despliegue)
8. [Presentación (slides)](#8-presentación-slides)
9. [Vídeo](#9-vídeo)
10. [Usuario y contraseña de prueba](#10-usuario-y-contraseña-de-prueba)

---

## 1. Descripción general

Los juegos de naves espaciales suelen ponerte en el puesto del capitán. MOP-13 cuenta la misma crisis desde abajo:
eres **MOP-13**, el conserje del turno de noche, y tu trabajo es que la nave esté limpia a las 06:00 mientras
todo se rompe a tu alrededor.

Cada turno empieza con un memorándum que asigna las tareas de la noche (fregar una sala hasta un porcentaje,
vaciar papeleras, eliminar residuos…). A partir de ahí la nave se simula sola: los tripulantes comen, duermen, van
al baño y trabajan, y por el camino ensucian, rompen cosas y provocan incidentes. Hay incendios, fugas de oxígeno,
plagas, apagones, inspectores corporativos y criaturas en la ventilación. Al final del turno recibes un informe con
créditos y experiencia para mejorar al conserje.

El tono es de comedia absurda («Si algo arde, puede ayudar. Si algo se mueve y es verde, no lo moje»), pero el
juego tiene sistemas reales: una simulación de necesidades para cada tripulante, emergencias encadenadas,
herramientas con carga y desgaste, progresión persistente y un multijugador cooperativo online con voz.

| Menú principal | Multijugador cooperativo (2 conserjes en la misma nave) |
|---|---|
| ![Menú principal](docs/img/menu.jpg) | ![Dos conserjes en la misma partida](slides/composition/img/coop.jpg) |
| **Mapa de la nave** | **Taquilla de mejoras** |
| ![Mapa de la USC Desastre](slides/composition/img/map.jpg) | ![Taquilla de mejoras](slides/composition/img/shop.jpg) |

## 2. Funcionalidades principales

### La nave
- **8 salas y un pasillo central**: cabina de mando, comedor, baños públicos, laboratorio científico, ingeniería,
  dormitorios, enfermería y jardín hidropónico. El minimapa (`M`) muestra el porcentaje de limpieza de cada sala.
- **12 tipos de tripulante** (capitán, científica, ingeniero, cocinero, turista espacial, inspector corporativo,
  médica, robot de mantenimiento, seguridad, diplomático alienígena, pasajero paranoico y contrabandista).
  Cada uno tiene necesidades (hambre, fatiga, vejiga, trabajo y vida social) y elige qué hacer para cubrirlas.
- **29 tipos de incidente** en cuatro categorías:
  - *Cotidianos*: vómitos, bandejas al suelo, inodoros atascados, tuberías con fugas, la cafetera que explota…
  - *Técnicos*: cortocircuitos, incendios eléctricos, fugas de oxígeno, pérdida de gravedad, reactor sobrecalentado, apagón general.
  - *Invasiones*: Rasgadores, brotes de enfermedad, moho, roedores de chatarra, enjambres devoracables, babosa gigante…
  - *Absurdos*: el capitán organiza una fiesta, una tormenta espacial, la IA de la nave declara una mancha obra de arte, los altavoces mienten…
- **Indicadores de la nave**: limpieza, integridad, energía, oxígeno, moral y contagio.

### Las herramientas (teclas 1–8)
| Tecla | Herramienta | Para qué sirve |
|---|---|---|
| 1 | Fregona básica | Limpia casi todo; se satura y deja el suelo mojado (la gente resbala) |
| 2 | Pistola de espuma selladora | Sella fugas y apaga fuegos |
| 3 | Aspiradora espacial | Sólidos y criaturas; hay que vaciar el depósito |
| 4 | Desinfectante industrial | Moho, contagios y curar tripulantes |
| 5 | Pulverizador criogénico | Congela lo que se mueve |
| 6 | Trampas para plagas | Roedores e insectos |
| 7 | Dron de limpieza | Aspira sólidos por su cuenta (mejora desbloqueable) |
| 8 | Superfregona experimental | El legado de MOP-1 (secreto) |

### Modos de juego
- **Historia**: 9 noches con guion propio y 3 jefes (el Glorp gigante, la Reina del Moho y el Rey Aspirador).
- **Turno infinito**: aguantar todo lo posible y batir el récord.
- **Limpieza zen**: sin emergencias graves.
- **Modo caos**: todo a la vez.
- **Desafíos** con reglas especiales: *Sin agua*, *A oscuras*, *Turno exprés*, *Solo fregona*…
- **Semilla de nave**: con la misma semilla se genera la misma nave.

### Progresión (se guarda en el navegador)
- **Créditos y experiencia** al final de cada turno.
- **Taquilla de mejoras**: 12 mejoras en 5 ramas (Limpieza, Tecnología, Supervivencia, Movilidad y Mantenimiento).
- **Especialización** a partir del nivel 5: conserje sanitario, técnico de emergencias, cazador de plagas o especialista antigravedad.
- **Vestuario**: 5 uniformes y 8 gorros que se desbloquean por nivel.
- **24 logros** y un **archivo secreto** con notas escondidas bajo la suciedad, un calcetín misterioso, un inodoro profético…

### Multijugador cooperativo online
- Hasta **4 conserjes** en la misma nave. El anfitrión crea una sala y comparte un **código de 5 letras** o un enlace `?sala=CÓDIGO`.
- El anfitrión elige el modo (Historia, Infinito, Zen o Caos). Quien entra más tarde aparece directamente en la nave.
- Si un conserje cae, Recursos Humanos envía un repuesto a los 10 s.
- Al terminar, todos reciben el mismo informe y cada uno cobra en su propia partida guardada.
- **Chat de voz** en la sala y en la pausa: micrófono abierto o pulsar para hablar (`V`), y voz por proximidad opcional.
- Si el alojamiento bloquea WebSocket, el juego cambia solo a sondeo HTTP.

### Controles
| Acción | Teclado |
|---|---|
| Moverse / correr | `W` `A` `S` `D` / `Shift` |
| Usar herramienta | Clic o `Espacio` |
| Cambiar herramienta | `Q` o `1`–`8` |
| Interactuar (mantener) | `E` |
| Tareas · mapa · pausa | `Tab` · `M` · `Esc` |
| Silenciar / hablar (multijugador) | `V` |

## 3. Stack tecnológico

| Capa | Tecnología | Detalle |
|---|---|---|
| Motor del juego | **Phaser 3.90** | Escenas, entrada, cámara y render en canvas/WebGL |
| Gráficos | **Pixel art generado por código** | Las texturas se dibujan al arrancar el juego; el build no contiene ni una imagen |
| Sonido | **Web Audio API** | Efectos sintetizados en el navegador |
| Interfaz | HTML + CSS | Menús y HUD encima del canvas; fuentes *Press Start 2P* y *VT323* |
| Empaquetado | **Vite** | Un bundle JS + CSS con hash en `dist/assets/` |
| Tiempo real | **WebSocket (RFC 6455) propio** + sondeo HTTP largo | Salas del multijugador, sin librerías |
| Voz | **WebRTC** | Audio directo entre navegadores; STUN público y TURN opcional |
| Servidor | **Node.js ≥ 18 sin dependencias** | Solo módulos nativos: `http`, `fs`, `path`, `zlib`, `crypto` |
| Persistencia | `localStorage` | Partida guardada, nombre de jugador y ajustes de voz |
| Despliegue | **Hostinger** (aplicación Node.js) | También preparado para VPS con PM2 + Nginx |
| Presentación | **HyperFrames 0.9.86** + GSAP | Slides hechas con HTML, navegables en el navegador |

## 4. Arquitectura

```mermaid
flowchart LR
    subgraph HOST["Navegador del anfitrión"]
        H["Phaser: simula la nave entera<br/>tripulación, criaturas, fuego, suciedad"]
    end
    subgraph SRV["Servidor Node.js (server.js)"]
        S1["Estáticos dist/ y slides/<br/>brotli, gzip, ETag"]
        S2["Salas /mp<br/>WebSocket o sondeo HTTP"]
        S3["/mp/ice: STUN y TURN"]
    end
    subgraph GUEST["Navegadores invitados"]
        G["Phaser: mueve a su conserje<br/>y pinta el estado recibido"]
    end
    H -- "tx: estado de la nave" --> S2
    S2 -- "rx" --> G
    G -- "up: entradas y acciones" --> S2
    S2 -- "up" --> H
    H <-. "voz WebRTC P2P (señalización 'sig' por el servidor)" .-> G
```

- **El anfitrión manda**: su navegador simula la nave completa y envía el estado a los invitados. Los invitados
  solo envían sus entradas y el anfitrión aplica sus herramientas, así que la suciedad que friega uno desaparece para todos.
- **El servidor solo reenvía mensajes** entre los miembros de cada sala (`create`, `join`, `tx`, `up`, `sig`, `bye`,
  `ping`). No guarda nada: las salas viven en memoria.
- **Dos transportes**: WebSocket implementado a mano sobre `http` (con ping cada 25 s y límite de 4 MB por mensaje) y,
  como alternativa, sondeo largo HTTP en `/mp/open`, `/mp/send`, `/mp/recv` y `/mp/close`.
- **Servidor de estáticos** con compresión brotli/gzip en memoria, `ETag`/304, caché inmutable para `assets/`,
  cabeceras de seguridad (`nosniff`, `SAMEORIGIN`, `Permissions-Policy`) y bloqueo de rutas que intentan salir de `dist/`.
- **Endpoints de servicio**: `/healthz` (`ok · salas N`) y `/mp/ice` (servidores ICE para la voz, configurables con
  `STUN_URLS`, `TURN_URLS`, `TURN_USER` y `TURN_PASS`).

## 5. Instalación y ejecución

**Requisitos:** Node.js 18 o superior. No hay dependencias que instalar.

```bash
git clone https://github.com/nastxp/Mop13.git
cd Mop13
npm start
```

Abre http://localhost:3000. Para probar el multijugador en local, abre una segunda ventana (o un navegador
distinto), pulsa **Multijugador cooperativo → Unirse** y escribe el código de la sala.

| Variable | Para qué | Por defecto |
|---|---|---|
| `PORT` | Puerto HTTP | `3000` |
| `HOST` | Interfaz de escucha | `0.0.0.0` |
| `MP_QUIET` | `1` desactiva los mensajes de las salas en consola | — |
| `STUN_URLS`, `TURN_URLS`, `TURN_USER`, `TURN_PASS` | Servidores ICE para el chat de voz | STUN públicos de Google |

Comprobaciones rápidas:

```bash
curl http://localhost:3000/healthz   # → ok · salas 0
curl http://localhost:3000/mp/ice    # → {"iceServers":[...]}
```

> El chat de voz necesita HTTPS (o `localhost`) porque el navegador solo da acceso al micrófono en contextos seguros.

## 6. Estructura del proyecto

```
Mop13/
├── server.js                  ← servidor Node.js: estáticos, salas multijugador (/mp), /healthz, /mp/ice y /slides/
├── package.json               ← "npm start" · Node ≥ 18 · sin dependencias
├── dist/                      ← el juego compilado (Phaser + código del juego empaquetado con Vite)
│   ├── index.html
│   └── assets/
│       ├── index-CAV77TPe.js
│       └── index-GG4KET3K.css
├── slides/                    ← presentación del TFM hecha con HyperFrames
│   ├── index.html             ← mazo navegable (reproductor + controles de HyperFrames)
│   ├── composition/           ← la composición: 13 escenas, fuentes, capturas y GSAP
│   ├── vendor/                ← reproductor de HyperFrames 0.9.86
│   ├── MOP-13-presentacion.pdf
│   └── package.json           ← scripts "present" y "check" de HyperFrames
├── docs/
│   ├── DESPLIEGUE.md          ← guía detallada de despliegue (Hostinger y VPS)
│   └── img/
└── README.md
```

## 7. Despliegue

El juego está desplegado en **Hostinger** como aplicación Node.js:

- **URL:** https://khaki-elk-914724.hostingersite.com
- **Estado del servidor:** https://khaki-elk-914724.hostingersite.com/healthz
- **Archivo de entrada:** `server.js` · **comando de inicio:** `npm start` · **build:** vacío (el juego ya viene compilado)
- Hostinger asigna el puerto con la variable `PORT`.

La guía paso a paso (hPanel, VPS con PM2 + Nginx, cabeceras para WebSocket y TURN para el chat de voz) está en
[docs/DESPLIEGUE.md](docs/DESPLIEGUE.md).

## 8. Presentación (slides)

- **Online:** https://khaki-elk-914724.hostingersite.com/slides/ (la sirve el mismo `server.js`)
- **PDF:** [slides/MOP-13-presentacion.pdf](slides/MOP-13-presentacion.pdf)
- **En local:** con `npm start`, abre http://localhost:3000/slides/

Las slides están hechas con [HyperFrames](https://hyperframes.heygen.com): cada slide es una escena HTML de
1920×1080 dentro de una composición con línea de tiempo GSAP, y el componente `<hyperframes-slideshow>` añade la
navegación. Se avanza con las flechas o la barra espaciadora, y la tecla `P` abre la vista de público con notas
del presentador. Para editarlas o presentarlas con el servidor de HyperFrames:

```bash
cd slides
npm install
npm run present   # modo presentador
npm run check     # validación de HyperFrames
```

## 9. Vídeo

_Pendiente: añadir aquí la URL pública del vídeo con la explicación del proyecto y la captura de pantalla._

## 10. Usuario y contraseña de prueba

El juego **no tiene login**. Basta con abrir la URL y pulsar **Historia · Noche 1**. La partida se guarda en el
navegador de cada jugador (`localStorage`) y el servidor no guarda ningún dato.
