# DECISIONES FORJA (cerradas)

TODO: Dividir cada sección en un `ADR-*` independiente

Última actualización: 2026-10-31

**Léelo completo al empezar cada sesión. No reabras estas decisiones.** Si algo es ambiguo: elige lo más simple, déjalo como parámetro en el archivo de configuración y no preguntes, salvo bloqueo real.

---

## 1. Entorno

- Minecraft **1.20.1**, Forge **47.4.10**, Java **17**.
- Sin macros de función (llegaron en 1.20.2) ni componentes de datos (1.20.5+): usar formato NBT de 1.20.1.
- Carpeta de juego: `C:\ForjaEvento\minecraft`
- Mundo: `C:\ForjaEvento\minecraft\saves\world`. Todo el evento ocurre en `minecraft:the_nether`.
- Datapack: `...\saves\world\datapacks\evento_gladiadores` (namespace `torneo`). **Es la fuente de verdad.** El repositorio de GitHub (`juanrmos/Forja`) está desactualizado y puede diferir.
- Scripts nuevos: `C:\ForjaEvento\minecraft\kubejs\server_scripts\`
- Logs: `logs\latest.log` y `logs\kubejs\server.log`
- Arquitectura: **datapack + KubeJS**. Migración gradual: la lógica vieja sigue funcionando hasta que cada pieza se reemplaza. Descartados: plugin Paper, mod Forge propio en Java, Paper + MythicMobs.

## 2. Mods (lista cerrada, no añadir ninguno)

- L_Ender's Cataclysm 3.16 (mod central: jefes y equipo Ignitium)
- Cataclysm Weaponery 2.0 (1.20.1)
- Lionfish API 2.4-Fix, GeckoLib 4.8.3, Curios 5.14.1, Architectury 9.2.14, Kotlin for Forge 4.12.0
- KubeJS 2001.6.5 (build 26) y Rhino 2001.2.3 (build 10)
- musync 1.9.5 (Forge 1.20.1)
- ModernFix, FerriteCore, Canary, spark 1.10.53

**Eliminados (el datapack todavía los referencia hasta la Fase 6):** Ice and Fire, Aether y Lost Aether Content, Aquamirae, Alex's Caves y Mobs, Mowzie's Mobs, Artifacts, Advanced Netherite, Aquaculture, Sophisticated Backpacks, Just Hammers, Better Archeology, Chisels & Bits.

## 3. Coordenadas con nombre oficial (dimensión Nether)

| Nombre | Coordenadas | Uso |
|---|---|---|
| `ESTADIO_INICIAL` | `2204 242 -683` | Llegada de todos al iniciar el evento, antes de los enfrentamientos |
| `LOBBY_ARENA` | `2204 138 -638` | Lobby de la arena: derrotados, fin de combate y punto de reaparición |
| `ESCENARIO_CENTRO` | `2204 127 -564` | Centro del escenario: espectadores durante el combate y campeón |
| `VESTIDOR_OESTE` | `2148 128 -564` | Vestidor del jugador 1 en PvP |
| `VESTIDOR_ESTE` | `2256 128 -564` | Vestidor del jugador en PvE y del jugador 2 en PvP |
| `TIENDA` | `2204 200 -564` | Tienda final |

- Estas coordenadas salen del zip de GitHub. **Verificarlas contra el datapack instalado.**
- Los puntos de aparición de los jefes (alrededor de `2200–2208, 138, -630` y `2202–2206, 128, -564`) se extraen del código y se centralizan también.
- **Las coordenadas se escriben solo en el archivo de configuración.** Todo lo demás usa los nombres.

## 4. Modos de juego y punto de reaparición

PvE y PvP usan **la misma lógica**:

| Momento | Quién | Modo | A dónde |
|---|---|---|---|
| Lobby y espera antes del sorteo | Todos los participantes | Aventura | `LOBBY_ARENA` (el inicio del evento conserva la llegada a `ESTADIO_INICIAL`) |
| Se sortea | Combatiente PvE o ambos combatientes PvP | Supervivencia | Vestidor correspondiente |
| Durante el combate | Participantes no combatientes | Espectador | `ESCENARIO_CENTRO` |
| Termina cada combate | Combatientes y espectadores | Aventura | `LOBBY_ARENA`; el siguiente sorteo cambia al nuevo combatiente a supervivencia |
| Desconexión y reconexión durante una fase | Combatiente según la ronda; demás participantes según lobby o combate | Según el estado actual | El destino que corresponda al estado, no el punto por defecto |

- El cambio a supervivencia ocurre al entrar al vestidor. Los espectadores cambian a espectador al comenzar el combate.
- Al conectarse o reaparecer, aplicar el modo que corresponda al estado actual del evento.
- **Aventura no impide que los jugadores se golpeen.** Cancelar el daño entre jugadores, salvo entre `jugador1` y `jugador2` durante un duelo PvP.
- **Punto de reaparición:** al iniciar el evento (cuando se teletransporta a `ESTADIO_INICIAL`) fijar el punto de reaparición de cada jugador en `LOBBY_ARENA`, ejecutándolo en el Nether. Repetirlo para quien se conecte tarde. Así, al morir no vuelven al Overworld. Comprobar en el juego que funciona (`VERIFICAR`: la dimensión del punto de reaparición).
- Antes de implementar, revisar si el datapack instalado o bloques de comandos del mundo ya cambian modos, para evitar choques.
- Decisión abierta menor: si la supervivencia permite romper bloques de la arena, decidir si se bloquea.

## 5. Timers

- Archivo de configuración con interruptor `MODO_PRUEBA`.
- En prueba: **todos los timers de flujo duran 5 s**. El timer de combate tiene su propio valor aparte (por ejemplo 60 s) y un comando de administrador para terminar el combate a mano.
- Valores reales: lobby 180 s, vestidor y apuestas 30 s, combate PvE 180 s, combate PvP 300 s, intermisión 120 s.
- Nunca escribir los tiempos dentro de la lógica: siempre desde la configuración.

## 6. Clases de equipo

**Decisión provisional nueva (2026-10-31):** durante las pruebas, las cuatro clases y sus variantes `sala2` reciben el mismo equipo: armadura completa de diamante, espada de diamante, 7 manzanas doradas y 4 perlas de Ender. Las cantidades de consumibles proceden de los ocho sets antiguos. Esta decisión reemplaza temporalmente las diferencias de clase descritas abajo; se retomarán al cerrar la Fase 6.

- **Fuego:** armadura Ignitium + Incinerator + Bulwark of the Flame (Cataclysm), con atributos normalizados. Buscar los ID reales en el jar, no adivinar.
- **Hielo, Trueno, Ejecutor:** equipo vanilla (netherite o diamante) con *trims* y colores distintos, y efectos propios en KubeJS (hielo: lentitud; trueno: rayo; ejecutor: hacha de daño alto).
- Las 4 clases tienen el **mismo presupuesto de estadísticas**.
- Escudos: vanilla con estandarte de color por clase.
- Cada clase tiene variante `sala2` (se conserva).

## 7. Jefes

- Reserva de 5 jefes de Cataclysm: Aptrgangr, Ender Golem, Kobolediator, Prowler y Watcher, Wadjet.
- **2 jefes por jugador** (constante configurable). Listas según jefes derrotados: `lista0` (0), `lista1` (1), `lista2` (2).
- Datos de cada jefe (identificador de entidad, atributos, orden) en una tabla de configuración.

## 8. Apuestas (solo con puntos, sin dinero real)

- **Regla legacy de pago fijo**: al apostar se descuentan puntos; si acierta, el apostador recibe el doble de lo que apostó (incluye la devolución de su apuesta más una ganancia igual). Si pierde, pierde lo apostado. El sistema puede crear puntos, igual que el legacy.
- Puntos iniciales: 200 por jugador.
- Montos: 50, 100 y all-in. PvE: a favor del jugador o de los jefes. PvP: jugador 1 o jugador 2.
- Se cierran al empezar el combate.
- Si un bando queda sin apuestas, **no hay devolución por ese motivo**: quien apostó al bando perdedor pierde su apuesta; no hay ganadores que cobrar. Empate: se reembolsan todas las apuestas.

## 9. Tienda

- Se mantienen estructura y precios.
- Los ítems de mods eliminados se reemplazan **1:1** por equivalentes vanilla en una tabla de configuración.
- Las 6 cajas de mods se reducen a 1 o 2 cajas con ítems vanilla y de Cataclysm. Sin accesorios de Artifacts.

## 10. Música y sonido

- musync 1.9.5. No usar `stop` seguido de `play` (corte brusco): usar `queue` y `skip`.
- Efectos con `playsound` en una categoría con volumen explícito (≈0.3 a 0.5), no `master` sin volumen.
- Normalizar el volumen de las pistas.

## 11. Mundo

- No se tocan regiones ni dimensiones. La decoración de mods eliminados se acepta como perdida y se revisa al final.

## 12. Reglas de trabajo

- **Una fase por sesión.** Entregar archivos completos con su ruta, y actualizar `ESTADO.md` al terminar.
- El usuario no programa: instrucciones en español sencillo, un paso a la vez, diciendo en qué carpeta va cada archivo.
- Todo lo que no se sepa seguro de KubeJS 6 se marca `VERIFICAR`.
- Ahorro de tokens: no releer las funciones del datapack, usar el CSV de la Fase 0; pedir solo las líneas `ERROR` de los logs; devolver solo los archivos modificados.

## 13. Criterios de aprobación

| Fase | Se aprueba cuando |
|---|---|
| 0 | El juego carga, `/reload` no da errores nuevos y KubeJS carga sin errores |
| 1 | Los comandos de administrador responden y el estado cambia |
| 2 | Lobby, modos de juego y reaparición funcionan; sin golpes de volumen |
| 3 | Varios jugadores pasan el PvE sin repetirse y quedan en la lista correcta |
| 4 | Se confirma el pago doble legacy: acierto recibe 2× apuesta, fallo pierde apuesta y empate devuelve la apuesta |
| 5 | Un torneo PvP completo funciona |
| 6 | Cada clase, compra y logro funcionan |
| 7 | Servidor local aguanta la prueba de carga con `spark` |

## 14. Entidades de prueba para PvE, PvP y apuestas

- La función instalada `data/torneo/functions/fase3/debug.mcfunction` limpia aldeanos con `participante` y crea seis aldeanos llamados `Aldeano_A` a `Aldeano_F`: dos llevan `participante,lista0`, dos `participante,lista1` y dos `participante,lista2`. Los invoca en `LOBBY_ARENA`.
- Esta plantilla prueba el sorteo PvP porque esas funciones seleccionan entidades con `@e` y etiquetas de lista.
- **Limitación comprobada:** el sorteo y el flujo PvE seleccionan participantes con `@a`, que solo incluye jugadores. Los aldeanos no pueden probar actualmente ese flujo. Los comandos de apuestas y sus scoreboards también apuntan a `@a`; los aldeanos no tienen `puntos`, `apuesta_j1` ni `apuesta_j2`, así que tampoco prueban apuestas ni su resolución.
- Al refactorizar, crear una herramienta de prueba KubeJS que permita escoger el tipo de participante. Mantener jugadores reales para verificar la experiencia y una opción de NPC para pruebas repetibles. La selección y el cálculo de apuestas deben compartir la misma lógica para jugadores y NPC de prueba, o declarar claramente qué casos solo se validan con jugadores.
- Etiquetar los NPC de prueba con un tag exclusivo, por ejemplo `forja_test`, además de `participante` y su lista. La limpieza debe afectar solo NPC con `forja_test`; no matar entidades reales solo por compartir `participante`.
- Incluir un comando de administrador para finalizar una prueba matando al jefe o participante de prueba, por ejemplo `/forja prueba matar jefe` o `/forja prueba matar jugador`. Debe actuar solo sobre entidades creadas por el modo de prueba y activar el mismo evento de muerte, cálculo del resultado y resolución de apuestas que el flujo normal. No resolver apuestas directamente desde el comando ni afectar entidades reales.
- Usar los aldeanos para probar composición de listas y emparejamiento. Para simular daño, muertes, desconexiones, reaparición o apuestas, comprobar primero que la entidad de prueba reciba el mismo estado y pase por las mismas reglas que una persona. No dar esos casos por probados solo porque el aldeano aparezca o sea emparejado.
