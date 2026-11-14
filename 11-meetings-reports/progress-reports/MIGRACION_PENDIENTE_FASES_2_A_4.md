# Inventario de migración pendiente: fases 2 a 4

Fecha: 2026-10-31. Comparación estática del datapack instalado en `minecraft/saves/world/datapacks/evento_gladiadores/data/torneo/functions/` con `minecraft/kubejs/server_scripts/00_forja_config.js`, `10_forja_lobby.js` y `20_forja_pve_apuestas.js`. **No se ejecutaron pruebas dentro del juego para este inventario.**

**Actualización de la corrección:** este inventario refleja el estado anterior a las correcciones del 2026-10-31. El código actual incorpora títulos, bossbars, música de vestidor y combate, anticampeo, salud visible, atributos de jefes, maniquíes, carteles clicables y mensajes de apuestas. La liquidación se cambió al pago fijo legacy (acierto 2×, fallo pierde, empate reembolsa). El cálculo está separado en `global.forjaBetPayouts` para poder reutilizarlo en PvP. También se ajustó el cambio de modos y destinos de reconexión para PvE. **Todo sigue pendiente de `/reload` y prueba dentro del juego; las filas siguientes son la auditoría original.** El estado vigente está en `ESTADO.md`.

La numeración de este documento es la del plan: **fase 2 = lobby; fase 3 = PvE (carpeta legacy `fase2`); fase 4 = apuestas (carpeta legacy `apuestas`)**. Se compara el comportamiento, no se exige copiar cada función antigua literalmente. Los elementos que dependían de mods eliminados requieren sustitutos válidos.

## Fase 2: lobby, llegada y audio

| Comportamiento legacy | Estado en KubeJS | Trabajo pendiente |
|---|---|---|
| Objetivos, 200 puntos, participantes, bossbar y cuenta regresiva (`fase1/iniciar`, `fase1/countdown`) | **Parcial**: puntos, tag, bossbar y cuenta de 5 s ya funcionan. | Revisar reinicio completo de tags y objetivos (`jugador1/2`, listas, descalificación, apuestas, muertes, logros) antes de una nueva edición; conservar datos si se reanuda tras `/reload`. Reproducir el título/subtítulo inicial y el estilo de bossbar si se desean iguales al evento anterior. |
| Playlist inicial (`musica/cargar_queue`, `musica/fase1_contador`) | **Parcial**: se encola y salta a la pista de lobby. | Planear la transición de pista al PvE y las siguientes fases sin `stop` seguido de `play`; revisar volumen real de las seis pistas. |
| Llegada, sonido diferido, limpieza del inventario y enlace a PvE (`fase1/teletransporte`, `fase1/sonido_llegada`) | **Parcial**: teletransporte, aventura, reaparición y efectos están probados. | Reproducir el título/subtítulo de llegada; decidir la limpieza de inventario y el momento del sonido. El PvE todavía requiere comando manual: no existe transición automática desde el lobby. |
| Protección contra golpes entre participantes (`DECISIONES.md`, §4) | **Falta**. | Cancelar daño jugador contra jugador fuera de un duelo PvP; aventura por sí sola no lo impide. |

La Fase 2 fue aprobada por el usuario para las pruebas realizadas. Los renglones pendientes registran **diferencias con el evento legacy o compromisos escritos en `DECISIONES.md`**; su prioridad debe acordarse antes de declarar migración íntegra.

## Fase 3: PvE

| Comportamiento legacy | Estado en KubeJS | Trabajo pendiente |
|---|---|---|
| Contar pendientes, sortear sin repetir y anunciar al combatiente (`fase2/iniciar`, `fase2/sorteo/*`) | **Parcial**: cola aleatoria y nombre en chat. | Título grande y subtítulo con nombre; manejo visible de jugadores que se desconecten durante la cola; retraso entre participantes. |
| Vestidor: puerta, música, sonido, bossbar de preparación y cuenta (`fase2/vestidor/*`, `sonidos/vestidores`, `musica/fase2_apuestas`) | **Parcial**: teletransporte, supervivencia, puerta este y temporizador. | Bossbar de vestidor, cuenta visible y sonidos de entrada/últimos segundos; pista `fase2_apuestas_vestidor` con `queue`/`skip`; verificar puerta oeste y toda la secuencia al entrar/salir. |
| Abrir puertas, grito de pelea y cerrar apuestas (`fase2/vestidor/abrir_puerta`, `fase2/combate/abrir_puerta`, `sonidos/vestidores_abrir`) | **Parcial**: se abre la puerta este y se bloquean apuestas por estado. | Sonido de apertura, título grande «PELEA»/«ANIQUÍLALOS», cerrar maniquíes y carteles, y conservar las posiciones de ambas puertas. |
| Cinco encuentros, invocación y atributos (`fase2/combate/sortear_jefe`, `fase2/combate/jefes/*`) | **Parcial**: están los cinco ID y ubicaciones, y se intenta evitar repetir encuentro consecutivo. | Migrar/configurar salud y atributos NBT del datapack; comprobar que el encuentro Prowler + dos Watchers se resuelva solo después de las tres muertes; registrar exactamente qué jefe murió. |
| Combate: música, bossbar de tiempo, últimos diez segundos, salud visible (`fase2/combate/iniciar`, `bossbar_*`, `vida/*`, `musica/fase2_jefe_tartaro`) | **Falta** casi todo: solo existe temporizador interno. | Pista `fase2_jefe_tartaro`, bossbar y actionbar, visualización de vida y limpieza de indicadores al terminar. El tiempo agotado debe anunciarse en pantalla. |
| Anticampeo (`fase2/anticamp/*`) | **Falta**. | Detectar salida del vestidor, colocar/retirar telarañas, descontar advertencias al regresar, mostrar contador privado y resolver derrota al agotarlo. Limpiar telarañas y estado incluso tras muerte, parada o `/reload`. |
| Victoria o derrota, sonido, puntos y clasificación (`fase2/combate/jefe_derrotado`, `primer_jefe_derrotado`, `segundo_jefe_derrotado`, `jugador_derrotado`, `sonidos/victoria_fase2`, `sonidos/derrota_fase2`) | **Parcial**: 0/1/2, +100 por jefe y apuesta resuelta. | Títulos/subtítulos de resultado, efectos, limpieza del equipo según la regla final, tags temporales, retorno al lobby y puerta, protección contra doble resolución por muertes simultáneas, reconexión y marcador de salud. La primera victoria legacy asignaba `lista1` temporalmente. |
| Logros de jefe, listas, maestro y cazador (`fase2/combate/jefe_derrotado`, `achievement/dar_*`) | **Falta**. | Otorgar logros con los mismos disparadores usando íconos válidos; varios advancements legacy aún hacen referencia a ítems de mods eliminados. |
| Final de todos los jugadores (`fase2/iniciar`, `jugador_derrotado`, `segundo_jefe_derrotado`, `intermision/inicio_fase3`) | **Parcial**: estado `PVE_TERMINADO`. | Intermisión real y enlace controlado a PvP cuando esa fase exista. |
| Pruebas con aldeanos (`fase3/debug`, decisión §14) | **Parcial**: un aldeano de prueba y comandos para matar jefe/jugador de prueba. | Verificar que la muerte del aldeano y de cada jefe active el mismo resultado y liquidación que un jugador; cubrir dos rondas y los tres resultados de lista. |

## Fase 4: apuestas PvE y base reutilizable para PvP

| Comportamiento legacy | Estado en KubeJS | Trabajo pendiente |
|---|---|---|
| Abrir apuestas y montar presentación (`apuestas/iniciar_fase2`) | **Falta** en el mundo: solo se abre el estado lógico. | Dos maniquíes en las posiciones existentes, con equipo válido y nombre visible; seis carteles clicables (50, 100 y todo, por cada bando), dos carteles de nombre y etiquetas que permitan limpiar solo los objetos de esta ronda. Reemplazar armaduras de mods eliminados. |
| Acciones de los carteles (`apuestas/apostar_50_*`, `apostar_100_*`, `apostar_allin_*`) | **Parcial**: `/apostar jugador|jefe 50|100|allin` valida saldo y una apuesta por ronda. | Conectar cada cartel nuevo al comando KubeJS correcto; informar al apostador con mensaje de confirmación/error; ofrecer camino accesible a quien no pueda usar carteles. |
| Cierre físico (`apuestas/cerrar`) | **Falta**: el estado lógico se cierra al iniciar combate. | Quitar maniquíes y los ocho carteles exactamente al cerrar; limpiar también en parada y fallo de ronda, sin borrar decoración ajena. |
| Resolución (`apuestas/resolver_j1`, `resolver_j2`, `resolver_empate`) | **Nuevo cálculo parcial**: pozo común con enteros, reembolso cuando falta un bando o hay empate; conserva el total en el cálculo. | Validar con apuestas reales, sin apuestas, un solo bando, empate, desconexión y muerte simultánea. Mostrar ganador, perdedor, devolución y saldo individual; limpiar el estado visible de la ronda. |
| Scoreboard y logros asociados (`apuestas/resolver_*`, `achievement/dar_buen_ojo`, `dar_primero`, `dar_en_cima`, `dar_donante`) | **Falta o parcial**: `puntos` se actualiza, pero no se crean/limpian `apuesta_j1` y `apuesta_j2`, ni se otorgan logros. | Unificar la fuente de verdad entre saldo KubeJS y scoreboard, incluida recarga/reconexión. Migrar logros cuando sus ítems de presentación sean válidos. |
| Reutilización para PvP (`apuestas/iniciar_fase3`, `f3_apostar_*`) | **Pendiente de Fase 5**. | Extraer el pozo/cálculo y la presentación común para dos lados configurables (`jugador`/`jefe` o `jugador1`/`jugador2`). El código actual mezcla el cálculo con estado y comandos exclusivos de PvE; no puede usarse en PvP sin refactor. |

## Orden para cerrar la migración

1. Corregir la compatibilidad de carga KubeJS y estabilizar el flujo de muerte/estado. El último error conocido de expansión de objeto fue corregido, pero falta confirmar `/reload` dentro del juego.
2. Completar el combate y el vestidor: bossbars, música, presentación, anticampeo, salud y limpieza.
3. Reponer los maniquíes, carteles y mensajes de apuestas; conservar el pozo y separar el cálculo para reutilizarlo en PvP.
4. Verificar en el juego cada caso de la tabla; mantener Fases 3 y 4 abiertas hasta entonces.

**Advertencia de alcance:** el datapack antiguo todavía contiene funciones y carteles que apuntan a ítems de mods eliminados. La nueva ruta no debe invocarlas directamente. Esta auditoría no cuenta como prueba de comportamiento en Minecraft.
