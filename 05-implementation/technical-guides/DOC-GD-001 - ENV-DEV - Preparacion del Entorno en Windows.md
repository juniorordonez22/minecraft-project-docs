# Guía de preparación del entorno de Forja

**Dirigida al equipo que desarrolla el mod.** Esta guía prepara Windows para la plataforma candidata: Minecraft 1.21.1, NeoForge 21.1 y Java 21. El entorno existente de Minecraft 1.20.1 usa Java 17. Las versiones concretas de NeoForge y de los mods aún necesitan validación conjunta.

La carpeta de trabajo propuesta es `C:\ForjaDesarrollo`. En otra computadora se puede elegir otra ruta, pero habrá que sustituir esa ruta en los comandos de esta guía. El proyecto actual sigue en `C:\ForjaEvento` en la computadora principal.

**Estado en la computadora principal:** Java 17 y 21 ya están instalados localmente; Git, VS Code y las extensiones ya estaban disponibles. La plantilla oficial se construyó y se inició su cliente. Para ver versiones y pendientes, consulta [ESTADO_ENTORNO_FORJA.md](ESTADO_ENTORNO_FORJA.md). Los demás integrantes deben seguir los pasos que aún no hayan realizado en sus propias computadoras.

## 1. Carpetas de trabajo

En la computadora principal se ha solicitado crear esta estructura:

```text
C:\ForjaDesarrollo\
  herramientas\java-17\
  herramientas\java-21\
  herramientas\gradle-cache\
  mod\
  pruebas\servidor-neoforge\
  docs\
```

Las carpetas vacías no instalan programas por sí solas. `mod` alojará el código fuente; `pruebas` es para pruebas aisladas; `docs` contiene la documentación. Las carpetas `minecraft` y `servidor_local` del proyecto actual permanecen en `C:\ForjaEvento`.

**En otra computadora:** abre el Explorador de archivos y crea la carpeta `C:\ForjaDesarrollo`. Dentro crea las subcarpetas mostradas arriba, una por una.

## 2. Antes de descargar

1. Comprueba que la computadora tenga acceso a internet y espacio suficiente para el JDK, Gradle, Minecraft, dependencias y mundos de prueba; la primera preparación puede descargar varios archivos y tardar. No se ha medido aún cuánto ocupará este proyecto.
2. Comprueba si Windows es **x64** o **ARM64**: abre **Configuración > Sistema > Acerca de** y busca **Tipo de sistema**. Descarga los archivos que coincidan con tu equipo. Las instrucciones de ejemplo siguientes usan x64; ARM64 necesita su paquete correspondiente.
3. Para probar el juego como jugador necesitarás Minecraft Java Edition y una cuenta con acceso al juego. Las pruebas de desarrollo también pueden usar el cliente de prueba del MDK cuando esté preparado.

## 3. Descargar Java 21

1. Abre la [página oficial de descargas de Microsoft OpenJDK](https://learn.microsoft.com/java/openjdk/download).
2. Busca **OpenJDK 21 LTS**.
3. En **Windows**, elige **x64 > zip**, o ARM64 si corresponde a tu equipo.
4. Descarga el ZIP a `Descargas`.
5. Haz clic derecho en el ZIP y elige **Extraer todo**. Abre la carpeta extraída.
6. Localiza la carpeta que contiene una carpeta llamada `bin`. Copia **su contenido** a `C:\ForjaDesarrollo\herramientas\java-21\`.
7. En el Explorador, comprueba que exista este archivo exacto:

```text
C:\ForjaDesarrollo\herramientas\java-21\bin\java.exe
```

Si ves `java-21\nombre-de-otra-carpeta\bin`, los archivos quedaron una carpeta demasiado adentro: mueve el contenido de esa carpeta interior a `java-21`. No elimines un Java ya instalado en la computadora. La guía del proyecto usará rutas explícitas para evitar cambiar la versión global de Windows.

## 4. Descargar Java 17 para la referencia

1. En la misma página oficial, busca **OpenJDK 17 LTS** y descarga el ZIP de Windows para la arquitectura de tu equipo.
2. Extráelo y copia el contenido de la carpeta que contiene `bin` a:

```text
C:\ForjaDesarrollo\herramientas\java-17\
```

3. Comprueba que exista:

```text
C:\ForjaDesarrollo\herramientas\java-17\bin\java.exe
```

Java 17 se reserva para ejecutar una copia compatible de la referencia 1.20.1. El servidor actual puede seguir usando su configuración existente. Java 21 es el que usaremos para construir el mod candidato.

## 5. Instalar VS Code y Git

1. Descarga e instala [Visual Studio Code](https://code.visualstudio.com/download).
2. Descarga [Git para Windows](https://git-scm.com/install/windows), eligiendo x64 o ARM64 según tu equipo. Ejecuta el instalador y acepta las opciones predeterminadas si no tienes otras preferencias.
3. Abre VS Code. En la barra izquierda pulsa el icono de **Extensiones**.
4. Busca **Extension Pack for Java**, publicado por Microsoft, e instálalo.
5. Busca **Gradle for Java**, publicado por Microsoft, e instálalo.
6. Cierra y vuelve a abrir VS Code para que reconozca las herramientas.

Git y VS Code se instalan para tu usuario o computadora. Sus binarios no se guardan dentro de la carpeta del proyecto ni se suben al repositorio.

## 6. Abrir la carpeta correcta

1. En VS Code, pulsa **Archivo > Abrir carpeta**.
2. Selecciona `C:\ForjaDesarrollo`.
3. Si VS Code pregunta si confías en la carpeta, comprueba que sea la carpeta de trabajo de Forja y acepta.

Cuando exista el proyecto del mod, se trabajará con `C:\ForjaDesarrollo\mod`. La estructura exacta del repositorio y de sus ramas se preparará antes de que el equipo comparta cambios; no copies manualmente la carpeta `.git` del proyecto antiguo.

## 7. Comprobar Java sin cambiar la configuración global

1. En VS Code, elige **Terminal > Nueva terminal**. Comprueba que la terminal sea **PowerShell**.
2. Copia este comando y pulsa Enter:

```powershell
& 'C:\ForjaDesarrollo\herramientas\java-21\bin\java.exe' -version
```

Debe aparecer una versión que empiece por `21`. Si aparece que el archivo no existe, revisa el paso 3 y la carpeta `bin`.

3. Copia este otro comando y pulsa Enter:

```powershell
& 'C:\ForjaDesarrollo\herramientas\java-17\bin\java.exe' -version
```

Debe aparecer una versión que empiece por `17`. Para comprobar el JDK de desarrollo completo, ejecuta:

```powershell
& 'C:\ForjaDesarrollo\herramientas\java-21\bin\javac.exe' -version
```

La versión también debe empezar por `21`.

## 8. Preparar el proyecto del mod

**Situación actual en la computadora principal:** la carpeta `mod` contiene la plantilla oficial y su `gradlew.bat`. Se trata del mod de ejemplo `examplemod`, todavía no de la lógica de Forja. En otra computadora, prepara u obtén primero el proyecto del equipo antes de ejecutar los comandos siguientes.

La base propuesta es el [MDK oficial de NeoForge 1.21.1 con ModDevGradle](https://github.com/NeoForgeMDKs/MDK-1.21.1-ModDevGradle). El equipo preparará el proyecto en `C:\ForjaDesarrollo\mod`, fijará las versiones elegidas y revisará su integración con el repositorio existente. Una descarga del MDK por sí sola es una plantilla de ejemplo: no es todavía el mod de Forja ni incorpora sus reglas.

Cuando recibas el proyecto preparado, comprueba que existan estos archivos:

```text
C:\ForjaDesarrollo\mod\gradlew.bat
C:\ForjaDesarrollo\mod\build.gradle
C:\ForjaDesarrollo\mod\settings.gradle
C:\ForjaDesarrollo\mod\src\
```

No hay que instalar Gradle por separado: el proyecto trae el **Gradle Wrapper**, que descargará la versión configurada cuando lo necesite. La primera ejecución necesita internet y puede tardar. El proyecto debe declarar y comprobar una toolchain de Java 21; la selección del Java que ejecuta Gradle y la del editor también deben verificarse.

## 9. Usar Java 21 y la caché local en PowerShell

Esta configuración afecta **solo a la terminal abierta**. En la computadora principal también existe el acceso `forja-gradle.ps1`, que selecciona el Java 21 y la caché locales para cada ejecución; las tareas de VS Code descritas en `ESTADO_ENTORNO_FORJA.md` lo utilizan.

1. En la terminal PowerShell de VS Code, ejecuta:

```powershell
$env:JAVA_HOME = 'C:\ForjaDesarrollo\herramientas\java-21'
```

2. A continuación, ejecuta:

```powershell
$env:GRADLE_USER_HOME = 'C:\ForjaDesarrollo\herramientas\gradle-cache'
```

3. Cuando exista el proyecto del mod, ve a su carpeta:

```powershell
Set-Location 'C:\ForjaDesarrollo\mod'
```

4. Comprueba qué JVM usa Gradle:

```powershell
.\gradlew.bat --version
```

Revisa que indique Java 21. El proyecto también debe comprobar que la **toolchain de compilación** seleccionada sea Java 21; que Gradle se ejecute con Java 21 no demuestra por sí solo qué JDK usará cada tarea. Si hay dudas, comparte la salida para revisarla.

`GRADLE_USER_HOME` coloca la caché de Gradle en la carpeta de herramientas. No convierte todo el sistema en un entorno virtual: VS Code, Windows y otros programas siguen teniendo sus propias configuraciones y cachés.

## 10. Construcción y pruebas iniciales

**Solo después de preparar el MDK para Forja y validar sus versiones:**

1. En la misma terminal y carpeta `C:\ForjaDesarrollo\mod`, ejecuta:

```powershell
.\gradlew.bat build
```

2. Si termina con `BUILD SUCCESSFUL`, busca el JAR generado en `C:\ForjaDesarrollo\mod\build\libs\`.
3. Para iniciar el cliente de desarrollo, usa:

```powershell
.\gradlew.bat runClient
```

4. Comprueba que el juego arranque y que el mod aparezca en la lista de mods. La primera vez puede tardar por la descarga de recursos.
5. El servidor de desarrollo se inicia con `.\gradlew.bat runServer` desde la misma carpeta. La plantilla actual usa `C:\ForjaDesarrollo\mod\run` para sus archivos de ejecución. En la computadora principal, la prueba arrancó sin solicitar EULA, con autenticación desactivada; se configuró `server-ip=127.0.0.1` y se verificó el acceso solo local. Una instalación dedicada distinta puede solicitar que se lea y acepte la EULA; la aceptación debe hacerla la persona responsable, después de leerla.

Estos comandos corresponden al flujo habitual de la plantilla oficial. El servidor de desarrollo con `online-mode=false` es solo para pruebas en la propia computadora; no lo expongas a internet. Los ajustes de `server.properties` de esta computadora no se transfieren automáticamente al resto del equipo. La parada normal desde la terminal de Gradle y la conexión entre cliente y servidor aún deben verificarse.

## 11. Git, documentos y archivos privados

- El código, la configuración del proyecto, el wrapper y la documentación se gestionan con Git cuando el repositorio de desarrollo esté preparado.
- Los JDK, `herramientas/gradle-cache`, `mod/build`, las carpetas de ejecución, mods descargados, copias del mundo y llaves deben quedar fuera del repositorio mediante reglas revisadas antes de subir cambios.
- Cada integrante obtiene sus herramientas desde sus fuentes oficiales. No copiar el JDK de Windows al servidor Linux: hay que usar el paquete correspondiente al sistema y arquitectura de cada equipo.
- El repositorio actual tiene automatización de despliegue a AWS. Antes de usar `main` o mezclar cambios, revisar los disparadores con la persona responsable y confirmar que no hay evento en curso.

## 12. Si algo falla

Anota qué paso estabas realizando y copia el mensaje completo que aparece en la terminal. Si Minecraft se abre pero falla al cargar, guarda `logs/latest.log` del **entorno que falló** e indica si era el cliente de desarrollo, servidor de desarrollo o servidor actual. Si el fallo pertenece al entorno antiguo KubeJS, adjunta además `logs/kubejs/server.log` cuando exista.

## Fuentes oficiales

- [NeoForge 1.21.1: requisitos, construcción y pruebas](https://docs.neoforged.net/docs/1.21.1/gettingstarted/)
- [MDK NeoForge 1.21.1 con ModDevGradle](https://github.com/NeoForgeMDKs/MDK-1.21.1-ModDevGradle)
- [Microsoft OpenJDK: descargas](https://learn.microsoft.com/java/openjdk/download)
- [Microsoft OpenJDK: instalación mediante ZIP](https://learn.microsoft.com/java/openjdk/install/)
- [VS Code: soporte para proyectos Java](https://code.visualstudio.com/docs/java/java-project)
- [Git para Windows](https://git-scm.com/install/windows)
- [Gradle: cachés y directorios](https://docs.gradle.org/current/userguide/directory_layout.html)
- [Gradle: toolchains Java](https://docs.gradle.org/current/userguide/toolchains.html)
