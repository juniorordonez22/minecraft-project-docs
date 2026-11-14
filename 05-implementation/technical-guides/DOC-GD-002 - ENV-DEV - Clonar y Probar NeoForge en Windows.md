# Guía para clonar y probar Forja NeoForge en Windows

Esta guía es para cada integrante que quiera obtener la rama de migración y arrancar **su propio servidor local de pruebas**. No conecta con AWS ni modifica el servidor actual de Forge/KubeJS.

> El repositorio de GitHub es privado. La persona debe tener acceso aprobado a `juanrmos/ForjaEvento`.
>
> Que el servidor arranque no significa que el torneo completo ya esté validado. Consulta [ESTADO.md](ESTADO.md) para ver qué recorridos siguen pendientes de prueba.

## Versiones del entorno

| Componente | Versión de trabajo |
| --- | --- |
| Minecraft | 1.21.1 |
| NeoForge | 21.1.265 |
| Java | JDK 21 |
| Rama Git | `migracion/neoforge` |
| Puerto local | 25565 |

La plantilla todavía conserva archivos de `examplemod`; el artefacto compilado puede llamarse `examplemod-1.0.0.jar`. Eso no significa que se deba instalar otra versión del torneo.

## 1. Instalar las herramientas

Se necesita Windows, Git para Windows, Java JDK 21 y Minecraft Java Edition. VS Code es opcional, pero facilita abrir el proyecto. Si todavía no tienes Java 21, descárgalo desde [Microsoft OpenJDK](https://learn.microsoft.com/java/openjdk/download) y extrae el JDK en:

```text
C:\ForjaDesarrollo\herramientas\java-21
```

Comprueba en el Explorador de archivos que exista `bin\javac.exe` dentro de esa carpeta. No hace falta instalar Gradle: el proyecto incluye Gradle Wrapper y prepara su propia caché.

## 2. Clonar la rama de migración

1. Abre **PowerShell** desde el menú Inicio.
2. Copia este comando y pulsa Enter:

```powershell
git clone --branch migracion/neoforge --single-branch https://github.com/juanrmos/ForjaEvento.git C:\ForjaDesarrollo\ForjaEvento
```

3. Si GitHub pide iniciar sesión, usa la cuenta que recibió acceso al repositorio privado.
4. Espera a que aparezca el mensaje de que terminó la descarga.

Este comando descarga todos los archivos versionados de esa rama. **No descarga mundos, mods en formato JAR, canciones ni archivos ignorados.** Si `C:\ForjaDesarrollo\ForjaEvento` ya existe, no lo borres; cambia el destino final del comando a una carpeta nueva, por ejemplo `C:\ForjaDesarrollo\ForjaEvento-prueba`.

Para abrir el proyecto, selecciona esa carpeta en **VS Code > Archivo > Abrir carpeta**. El proyecto que se compila está dentro de la subcarpeta `mod`.

## 3. Comprobar Java y construir el mod

1. En PowerShell, ejecuta:

```powershell
Set-Location 'C:\ForjaDesarrollo\ForjaEvento\mod'
```

Si usaste otro destino al clonar, reemplaza la ruta por la tuya.

2. Ejecuta:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\forja-gradle.ps1 --version
```

La salida debe mostrar Java 21. El script busca el JDK en `herramientas\java-21`, ya sea dentro de la carpeta clonada o en la carpeta de trabajo que la contiene. También guarda la caché de Gradle en `herramientas\gradle-cache`.

3. Construye el proyecto:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\forja-gradle.ps1 build
```

La primera ejecución descarga herramientas y dependencias; puede tardar y necesita internet. Al terminar debe aparecer `BUILD SUCCESSFUL`. El JAR se crea en `mod\build\libs`.

## 4. Preparar los mods del servidor

Los mods binarios no se guardan en Git. Descarga los JAR de NeoForge indicados en [Inventario de mods de prueba](INVENTARIO_MODS_NEOFORGE_PRUEBA.md) y colócalos en:

```text
C:\ForjaDesarrollo\ForjaEvento\mod\run\mods
```

Si las carpetas `run` o `mods` todavía no existen, créalas con el Explorador de archivos. No pongas un JAR dentro de otra carpeta ni guardes dos versiones del mismo mod.

**Atención con Lionfish API:** el inventario registra la versión 3.1, pero el último arranque del servidor de desarrollo que se revisó cargó la versión 2.7. Antes de probar, pide a la persona responsable el mismo JAR 2.7 que usó el servidor; ese archivo no está incluido en Git. No sustituyas ni mezcles versiones por tu cuenta.

## 5. Preparar canciones y mundo de prueba (opcional)

### Canciones

Para probar MuSync, coloca los seis archivos `.ogg` autorizados en:

```text
C:\ForjaDesarrollo\ForjaEvento\mod\run\customtracks
```

Los nombres esperados, sin la extensión, son `fase1_contador`, `fase2_apuestas_vestidor`, `fase2_jefe_tartaro`, `fase3_campeon_tienda`, `fase3_pvp` e `intermision`. El repositorio no incluye las pistas. La ubicación de las pistas dentro del directorio de juego del cliente depende de la instalación local de MuSync; **VERIFICAR** con su registro antes de probar música desde cliente.

### Arena

El repositorio no incluye ningún mundo. Sin una copia entregada por la persona responsable, el servidor genera un mundo nuevo y no tendrá la arena.

Para revisar la arena, pide una **copia de prueba completa del mundo**, no solo una carpeta de dimensión. Con el servidor apagado, coloca esa copia en `mod\run\world` y comprueba que `level.dat` quede directamente dentro de `world` (sin una carpeta `world` duplicada). Conserva el mundo original separado y no uses la copia de AWS como mundo de prueba.

En el mundo ya preparado, el terreno del Nether está en `world\DIM-1`.

## 6. Mantener el servidor accesible solo desde esa computadora

Antes de iniciar por primera vez, crea `mod\run` si aún no existe. Abre el Bloc de notas, copia las tres líneas de abajo y guarda el archivo como `server.properties` dentro de esa carpeta. En la ventana **Guardar como**, elige **Tipo: Todos los archivos** para que no se agregue `.txt` al final.

Comprueba que tenga estos valores:

```properties
server-ip=127.0.0.1
server-port=25565
online-mode=true
```

`127.0.0.1` limita la conexión a la computadora que ejecuta el servidor. No compartas ese puerto por internet ni cambies la dirección para abrirlo públicamente.

## 7. Arrancar el servidor

1. Abre PowerShell en la carpeta `mod`:

```powershell
Set-Location 'C:\ForjaDesarrollo\ForjaEvento\mod'
```

2. Inícialo:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\forja-gradle.ps1 runServer
```

3. Espera el mensaje `Done (...)! For help, type "help"`. La primera carga puede tardar. Si aparece un error, no sigas con los pasos de cliente: guarda `mod\run\logs\latest.log` y `mod\run\logs\debug.log` y compártelos con quien coordina la prueba.
4. Para apagarlo, escribe `stop` en la consola del servidor y espera a que finalice el guardado y Gradle indique que terminó. No cierres la ventana mientras Minecraft guarda el mundo.

## 8. Conectar Minecraft desde la misma computadora

Para probar con un cliente de Minecraft en la misma computadora:

1. En el Launcher, crea o selecciona una instalación de Minecraft **1.21.1 con NeoForge 21.1.265**.
2. Configura un directorio de juego separado, por ejemplo `C:\ForjaDesarrollo\cliente-neoforge`, para no mezclarlo con otra instalación. Inicia esa instalación una vez y cierra el juego para que el Launcher cree la carpeta.
3. Ejecuta `build` como se explicó arriba. En el Explorador, crea `C:\ForjaDesarrollo\cliente-neoforge\mods` y copia allí el JAR principal de `mod\build\libs` y los mismos JAR de dependencias del servidor.
4. Inicia el servidor y, sin cerrar su ventana, abre el Launcher y entra a Minecraft con ese perfil.
5. En **Multijugador > Conexión directa**, escribe:

```text
localhost:25565
```

Si un amigo ejecuta su propia copia local, cada uno repite estos pasos en su computadora. La dirección `127.0.0.1` no permite que otros equipos se conecten al servidor de tu computadora.

## 9. Comandos básicos de prueba

En la consola del servidor, da permisos de operador a tu usuario:

```text
op NombreDeUsuario
```

Dentro del juego, puedes consultar el estado del controlador:

```text
/forja_nativo estado
```

Para los comandos de prueba de PvE, apuestas, tienda y demás, sigue las guías enlazadas desde [ESTADO.md](ESTADO.md). Los comandos que simulan la muerte del jefe funcionan solo en las condiciones de prueba que describe la guía PvE; no matan jugadores ni entidades ajenas al encuentro.

## 10. Si algo falla

1. Copia el texto de error completo de la consola.
2. Adjunta `mod\run\logs\latest.log`; si existe, adjunta también `mod\run\logs\debug.log` y el reporte más reciente de `mod\run\crash-reports`.
3. Indica qué estabas haciendo y en qué paso.

Esta rama no usa los registros de KubeJS del servidor antiguo. No envíes llaves privadas, mundos completos ni JAR de mods al repositorio. El flujo NeoForge no despliega automáticamente a AWS; sigue siendo una prueba local.

## Lista rápida

- [ ] Cloné la rama `migracion/neoforge`.
- [ ] `--version` muestra Java 21.
- [ ] `build` termina con `BUILD SUCCESSFUL`.
- [ ] Coloqué las dependencias aprobadas en `mod\run\mods` y resolví la versión de Lionfish API con la persona responsable.
- [ ] Dejé el servidor en `127.0.0.1:25565`.
- [ ] El servidor llegó a `Done` y el cliente se conectó por `localhost:25565`.
- [ ] Apagué el servidor con `stop`.
