# Project Scope - Evento Forja

# Cuadro de Versiones

| **Versión** | **Fecha** | **Autor (es)** | **Descripción** |
|---|---|---|---|
| 1.0 | 3-11-2026 | Juan | Versión inicial del documento de alcance |


# 1. Alcance General del Evento

El desarrollo del evento está dividido y limitado únicamente a tres mecánicas:

- Rondas PvE: Selección aleatoria de jefes mediante ruleta, ajuste de vida y daño en tiempo real, y detección de victoria o derrota.
- Rondas PvP: Combates directos entre jugadores dentro de la arena, con zona de preparación en vestidores y sistema para evitar que los jugadores evadan el combate.
- Sistema de apuestas en paralelo: Mecánica para que los espectadores utilicen puntos durante los combates, con cálculo de pagos al finalizar cada ronda.

# 2. Limitaciones

- Está prohibido agregar nuevas fases, mecánicas o modalidades (como minijuegos, zonas de parkour o áreas adicionales) sin la previa consulta y aprobación del equipo responsable de cada área. 
- El objetivo del desarrollo es garantizar la estabilidad del servidor y evitar sobrecargar el flujo de trabajo.

# 3. Elección de Tecnologías y Plan de Migración

Para evitar fallos de conexión y optimizar el rendimiento del servidor y de los clientes, se establecen las siguientes decisiones técnicas:

- Mod propio en Java: Se migra la lógica actual de scripts (KubeJS) a código Java compilado dentro de un mod para Forge 1.20.1 (compatible con NeoForge). Esto reduce el consumo de procesador en el servidor.
- Recursos integrados en el archivo .jar: Las texturas, interfaces, sonidos y modelos se incluirán directamente dentro del archivo del mod. Con esto se elimina la necesidad de descargar paquetes de texturas por internet y se evitan los errores de descarga en los clientes.
- Modelado 3D y animaciones con GeckoLib: Se utiliza GeckoLib para integrar modelos propios y animaciones complejas, como la entidad del Ojo Angelical, vinculando sus movimientos a los eventos del servidor.
- Modificación de mods existentes mediante Mixins: En lugar de programar jefes desde cero, se descompilan y modifican los mods ya seleccionados (como Cataclysm) utilizando Mixins. Esto permite ajustar su inteligencia artificial, vida y comportamiento para adaptarlos a la arena sin rehacer el trabajo.
- Comunicación de red propia: Se implementa un canal de paquetes directos para enviar la información de la ruleta, barras de vida y temporizadores a las pantallas de los jugadores sin saturar la red.

# 4. Directrices para el Área de Diseño y Arte

- Coherencia temática: Todas las ilustraciones, modelos 3D y sonidos deben ajustarse estrictamente a la ambientación definida para el evento (temática de La Forja del Tártaro, coliseo y el Ojo Angelical).
- Estándares técnicos de entrega:
    - Texturas e interfaz: Archivos en formato .png optimizado y en dimensiones compatibles (potencias de 2 como 256x256, 512x512, etc.).
    - Modelado 3D: Archivos creados en Blockbench compatibles con GeckoLib, manteniendo una estructura de huesos limpia para no afectar los fotogramas por segundo.
    - Sonido: Archivos en formato .ogg. La música ambiental debe entregarse en estéreo y los efectos de sonido del mapa en mono para permitir la ubicación espacial dentro del juego.
- Coordinación con construcción: Las escalas y medidas de los modelos 3D deben coincidir con las dimensiones de la arena, gradas y vestidores construidos en el mapa.