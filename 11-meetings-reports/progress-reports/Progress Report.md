# ESTADO FORJA

Última actualización: 2026-11-01

Lee primero `Decisions.md`. Este archivo solo dice en qué punto está el trabajo. Actualízalo al terminar cada fase.

---

## Fases

- [x] **0. Parches e inventario**: aprobada por el usuario el 2026-10-31. Confirmó que `/reload` terminó correctamente y KubeJS mostró 0 errores.
- [x] **1. Esqueleto KubeJS**: comandos `/forja estado`, `/forja iniciar` y `/forja detener` probados por el usuario; KubeJS recarga correctamente.
- [x] **2. Lobby, audio y modos de juego**: aprobada por el usuario. Confirmó que el lobby, los modos, el punto de reaparición en las coordenadas del Nether y la música funcionan.
- [ ] **3. PvE**: corrección en curso. Logs del modo prueba confirmaron que el evento de muerte sí se ejecuta, pero la resolución se interrumpía por un error Rhino `redeclaration of var won`; corregido en el script actual. También se activó en prueba el teletransporte y modo espectador de participantes no combatientes. Pendiente volver a probar muerte de aldeano/jefe y retorno al lobby.
- [ ] **4. Apuestas**: corregidas para seguir el legacy: acierto paga 2×, fallo pierde lo apostado, empate devuelve. Reescribí los carteles con el formato NBT de los carteles legacy porque solo se veía el de 50. **VERIFICAR en el juego** que aparecen los seis carteles y que cada resultado paga como se describe. El cálculo fijo queda disponible para reutilizarse en PvP.
- [ ] **5. PvP**: módulo escrito con intermisión automática tras el PvE real, emparejamientos por listas, seis aldeanos de prueba, carteles de apuestas, anticampeo, empate, premio, campeón, reconexión y entrada a tienda. Equipos vanilla impiden golpes entre participantes fuera del duelo y separan a los dos combatientes PvP. Pendiente: cargar y probar dentro de Minecraft. Los comandos de prueba son `/forjapvp prueba` y `/forjapvp matar jugador1|jugador2`.
- [ ] **6. Clases, tienda y logros**: kit provisional instalado; catálogo de 36 productos con precios legacy y sustituciones vanilla, más dos cajas vanilla (50 y 100 puntos). Los carteles físicos se generan en las coordenadas legacy; los premios se compran con `/comprar`. Se corrigieron seis iconos de logros y se conectaron disparadores principales de PvE, PvP, apuestas y tienda. Pendiente: verificar cada compra y logro en Minecraft, revisar nombres visuales antiguos de productos y completar las clases definitivas cuando se retire la decisión provisional.
- [ ] **7. Pruebas y despliegue**: Forge 47.4.10 instalado en `servidor_local`, con mods, configuración, KubeJS, música y `server.properties`; instalador verificado por SHA1 oficial. El mundo cerrado se copió completo (1,213,959,108 bytes) y contiene el datapack. La EULA está aceptada. Arranqué el servidor y comprobé que KubeJS cargó 7/7 scripts con 0 errores y 0 advertencias; el servidor abrió el puerto local 25565 y respondió al comando de consola `list`. La cuenta `JuanRM7` quedó registrada como operadora. Quedan pendientes las pruebas dentro del juego con jugadores, una ronda completa, prueba con varios clientes y medición con spark. AWS sigue para después de validar localmente.

## Automatización de despliegue a AWS (2026-11-01)

- Se crearon `deploy.ps1`, `deploy.config.json`, `logs.ps1`, `rollback.ps1`, `DESPLIEGUE.md` y la tarea `.vscode/tasks.json`. `.gitignore` excluye `deploy.config.json` y cualquier archivo `.pem`. La configuración no se despliega.
- El despliegue normal solo reemplaza `world/datapacks/evento_gladiadores` y `kubejs/server_scripts`. `-Config` agrega `kubejs/config`; `-Mods` agrega `mods` y advierte que se requiere reiniciar. Antes de escribir, pregunta si hay un evento y cancela con respuesta afirmativa. Respaldos fechados: `/home/ubuntu/servidor/backups/forja/`.
- Pruebas ejecutadas: SSH con `echo ok`; `-DryRun`; script temporal KubeJS con marca de log; comparación de SHA-256; revisión de `world/level.dat` y mods; comprobación del proceso Java y tmux; rollback y despliegue final sin el archivo temporal.
- **Resultado:** conexión SSH aprobada. El ensayo `-DryRun` enumeró 223 archivos (incluía el archivo temporal) y no transfirió nada; el despliegue final enumeró y verificó 222 archivos. No hubo diferencias SHA-256 entre los archivos locales y remotos. La marca `[FORJA_DEPLOY_CHECK]` apareció en `logs/kubejs/server.log` después de iniciar y recargar KubeJS. El rollback restauró el respaldo y quitó la marca del árbol de scripts; luego se hizo el despliegue final.
- En el chequeo final, la fecha de modificación de `world/level.dat` se mantuvo exactamente en `2026-11-01 15:40:16.720098667 +0000` antes y después del despliegue final. Los 14 archivos de mods conservaron el mismo hash agregado (`f6e3576833992a23e0313674645ce0fb5cdfd296119a698f441b73f6587f8cc7`); no se usó `-Mods`.
- La sesión tmux `mc` y el proceso Forge/Java están activos. Al comenzar esta tarea, la sesión tmux existía pero mostraba una consola de Ubuntu: Minecraft ya estaba detenido. Se inició con `./run.sh nogui` después de confirmar que no había evento. Se corrigió `deploy.ps1` y `rollback.ps1` para exigir tanto tmux como el proceso Java, evitando enviar comandos de Minecraft a una terminal de Linux.
- **KubeJS actual:** 7/7 scripts, 0 errores y 0 advertencias en la recarga final. `logs/latest.log` aún muestra errores conocidos del datapack legacy: seis tablas de botín y las funciones `torneo:tienda/herramientas/comprar_guantes` y `torneo:tienda/comprar_netherite_gold`, que dependen de contenido retirado. No son errores de carga de KubeJS; quedan para la limpieza de la Fase 6.
- El archivo temporal `minecraft/kubejs/server_scripts/zz_deploy_check.js` se eliminó tanto local como remotamente. La última copia de respaldo corresponde al árbol limpio, sin ese archivo.
- La tarea **Desplegar a AWS** está en `.vscode/tasks.json`. `Ctrl+Alt+D` quedó agregado al archivo personal de atajos de VS Code de esta computadora; `.vscode/keybindings.json` conserva una copia para configurarlo en otra computadora. También se puede ejecutar desde `Tasks: Run Task`.

## Corrección de ingeniería inversa legacy vs KubeJS (2026-11-01)

- **Lobby e Intermisión (`10_forja_lobby.js`)**: se restauraron los títulos originales (`"La Forja del Tártaro"` y `"¡LA FORJA EXIGE UN CAMPEÓN!"`), el estilo `notched_10` en la barra del lobby, la limpieza de inventario (`clear`) al llegar al estadio, la limpieza completa de tags (`campeon`, `esperando`, `en_arena`, `apuestas_abiertas`, `combate_activo`) y el estado intermedio `LLEGADA_ESTADIO` (3 s) para que el título de llegada no se pise con la intermisión y los sonidos (`wither.spawn` y `soul_sand_valley`) suenen con el Nether ya cargado.
- **PvE y Apuestas (`20_forja_pve_apuestas.js`)**:
  - Se agregó la pausa de 5 s (`PVE_PAUSA`) entre el final del turno de un combatiente y el sorteo del siguiente para que se lean los títulos y subtítulos clásicos (`"¡Dominó la arena del Tártaro! -> Tercer Grupo"`, `"Dio mucha lástima... -> Primer Grupo"`, `"Ha caído... -> Segundo Grupo"`).
  - Se corrigió la apertura y cierre de **ambas** puertas (`PUERTA_OESTE` y `PUERTA_ESTE`).
  - Se restauró `Rotation:[180f,0f]` en los maniquíes de apuestas, armadura de Ignitium con nombre `"???"` en el maniquí del jefe, colores/negritas legacy en los carteles (`[ ALL IN ]` en `dark_red`), actualización de `apuesta_j1`/`apuesta_j2` y confirmaciones privadas por `tellraw`.
  - El marcador lateral `vida_combate` (`"❤ Vida"`) ahora muestra el nombre real del jefe (`Kobolediator`, `EnderGolem`, `Wadjet`, `Aptrgangr`, `ProwlerWatcher`) y se actualiza cada 4 ticks.
  - Se conectaron el logro `torneo:lista1` al vencer al primer jefe y `torneo:cazador_de_campeones` al derrotar a los 5 jefes, además de limpiar el inventario (`clear`) al terminar la participación del jugador.
- **Torneo PvP y Coronación (`30_forja_pvp.js`)**:
  - Se corrigió el bug de la cola de emparejamiento separando `data.queue` (ronda actual) y `data.winners` (ganadores esperando la siguiente vuelta) para que un ganador de `lista0` no vuelva a pelear inmediatamente en la pelea siguiente.
  - Se agregaron todos los sonidos faltantes (`evoker.prepare_summon` en vestidor, `ui.button.click` en los últimos 5 s, `event.raid.horn` al abrir puertas, `zombie_villager.cure` al ganar, `respawn_anchor.deplete`/`elder_guardian.curse` al perder/empatar, `ui.toast.challenge_complete` + `ender_dragon.death` en coronación y `wither.spawn` en `"NADIE ES DIGNO..."`).
  - Se añadieron `Rotation:[180f,0f]`, `ShowArms:1b` y armaduras a los maniquíes de apuestas PvP, mensajes `tellraw` de liquidación en `pvSettle`, marcador de vida en tiempo real (`vida_combate`), cuenta regresiva en `actionbar` en los últimos 10 s y limpieza de inventario (`clear`) al finalizar cada duelo.
- **Tienda (`40_forja_tienda.js` y datapack)**: se agregaron los sonidos de apertura (`item.totem.use` y `block.bell.resonate`), sonido al comprar (`experience_orb.pickup`), se eliminó el spam de 38 líneas en el chat global al abrir la tienda y se reemplazaron los dos ítems de mods retirados que quedaban en `comprar_guantes.mcfunction` y `comprar_netherite_gold.mcfunction`. Todo quedó sincronizado también en `servidor_local`.

Las fases 1 y 2 se hacen juntas.

## Resultado de la Fase 0

- El filtro del sorteo ya usa `fase2superada`. Sin cambios.
- `achievement/dar_ender_golem.mcfunction` existe y es idéntico a la copia con nombre duplicado; esa copia se renombró a `.disabled`.
- KubeJS cargó 1 de 1 scripts con 0 errores (solo el script de ejemplo).
- Inventario: 189 funciones revisadas. **335 referencias a 280 identificadores de mods eliminados en 43 archivos.** El CSV generado por Codex (ver su reporte de Fase 0) tiene archivo, línea, ID y comando.
- El último arranque registró **30 funciones y 6 tablas de botín** que no cargaron (usan ítems de mods eliminados).

## Preparación del servidor local (por Codex)

- `level.dat`: comandos activados (`allowCommands = 1`), creativo como modo predeterminado.
- El servidor dedicado tiene `allow-flight=true`, bloques de comando habilitados, PvP activo y aventura como modo predeterminado.
- El servidor conserva el mundo copiado; no se tocaron las regiones ni `DIM-1`.
- `run.bat` usa directamente Java 17 para evitar el lanzador `java.exe` antiguo que no iniciaba correctamente el servidor.
- La guía de arranque está en `servidor_local/COPIAR_MUNDO.md`.

## Respaldos

- Zip del mundo original: `mundo_original_NO_TOCAR` (del usuario). **No tocar.**
- Respaldo de Codex antes de la Fase 0: ruta en su reporte. Contiene el datapack, `level.dat` y datos de jugadores (no el mundo completo).

## Avisos aceptados (no son errores nuevos)

- Las 30 funciones y 6 tablas de botín de arriba: se arreglan en la Fase 6.
- Referencias del mundo a registros retirados y a la dimensión de Aether.
- Avisos sobre el atributo `obscure_api:regeneration`.

## Avisos por investigar

- **Recursos de Cataclysm faltantes** en el log: se revisó el JAR instalado y no contiene `maledictus_leap.ogg` ni `maledictus_idle3.ogg`. Es un problema del paquete del mod, independiente de los scripts de KubeJS.

## Próximo paso

1. **Fase 0 aprobada**: el usuario confirmó que `/reload` terminó correctamente y KubeJS mostró 0 errores.
2. **Fase 1 aprobada por el usuario**: comandos de administrador y estados probados.
3. **Fases 1 y 2 aprobadas.** El usuario confirmó comandos, lobby, reaparición dentro de las coordenadas del Nether y música.
4. **Fases 3 y 4 siguen abiertas.** Se repusieron los elementos visibles principales del datapack antiguo en KubeJS, pero todavía no se han probado tras esta corrección. El inventario original de diferencias está en `MIGRACION_PENDIENTE_FASES_2_A_4.md`.
5. Unirse al servidor local con `localhost` y revisar las fases desde dentro del juego. La cuenta `JuanRM7` ya tiene permisos de administradora. La guía está en `servidor_local/COPIAR_MUNDO.md`.
6. Las fases 3 a 6 ya están implementadas en KubeJS, pero siguen sin aprobación funcional hasta probar muertes, apuestas, PvP, compras y logros dentro del juego. El arranque limpio confirma que no hay errores de carga; no sustituye esas pruebas de jugabilidad.

## Corrección de armaduras (2026-10-31)

- Las ocho funciones `setsarmor/clase_*` ahora seleccionan al jugador más cercano dentro de la zona del vestidor y ejecutan la función común como ese jugador. El kit común equipa casco, pechera, pantalones y botas de diamante vanilla, además de espada, 7 manzanas doradas y 4 perlas.
- La entrega automática de KubeJS para combatientes PvE y PvP aplica el mismo kit bajo el contexto explícito del jugador en el Nether.
- Las funciones legacy de compra de armaduras ya no piden objetos de Ice and Fire, Aquaculture ni Advanced Netherite: sus identificadores se cambiaron por los equivalentes que ya usa `01_forja_catalogo.js` (diamante o netherita). Ignitium de Cataclysm se conserva.
- El servidor local tiene estos cambios sincronizados. KubeJS recargó 7/7 scripts sin errores ni advertencias. Sigue pendiente confirmar visualmente la armadura durante una ronda en Minecraft.

## Corrección de error y flujo completo (2026-10-31)

- La imagen mostraba un error de Rhino en `20_forja_pve_apuestas.js`: la variable local `slot` se declaraba dentro de cada vuelta del bucle de equipo. Se cambió por una lista de ranuras declarada una sola vez (`armorSlotNames`) y un índice persistente (`armorSlotIndex`).
- `/forja iniciar` antes iniciaba solo el lobby y dejaba el evento esperando que alguien lanzara PvE. Ahora, al terminar el lobby, entra en `INTERMISION_PVE` con barra, música y cuenta regresiva; al terminar inicia PvE automáticamente.
- `/forja pve iniciar` y `/forja pve prueba` siguen disponibles para administradores y adelantan la intermisión si se ejecutan durante ella.
- Después del PvE real se mantiene la intermisión automática hacia PvP; después, continúan los duelos, coronación y apertura de tienda. En prueba los tiempos de flujo están reducidos a 5 segundos; el combate conserva su timer separado.
- El servidor local recargó 7/7 scripts de KubeJS con 0 errores y 0 advertencias. Falta verificar dentro del juego el equipamiento y el recorrido automático completo.
- `/forjapvp prueba` ahora también puede iniciar con el evento `DETENIDO`, además de `LOBBY_ARENA` o `PVE_TERMINADO`, para permitir una prueba PvP aislada. Crea seis aldeanos sin IA; durante cada duelo se puede simular el resultado con `/forjapvp matar jugador1` o `/forjapvp matar jugador2`. KubeJS recargó 7/7 scripts sin errores ni advertencias.

