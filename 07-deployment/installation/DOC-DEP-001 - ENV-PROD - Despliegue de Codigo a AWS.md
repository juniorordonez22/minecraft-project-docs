# Despliegue de código de Forja a AWS

Este procedimiento copia solo el datapack `evento_gladiadores` y `kubejs/server_scripts`. No copia el mundo ni los mods por defecto. La IP, el usuario, la llave y la ruta remota están en `deploy.config.json`; ese archivo y cualquier `.pem` están excluidos de Git.

## Desplegar

1. Abre la carpeta `C:\ForjaEvento` en Visual Studio Code.
2. Ejecuta primero `powershell -ExecutionPolicy Bypass -File .\deploy.ps1 -DryRun` para revisar la lista de archivos. No sube nada.
3. Cuando estés listo, ejecuta `deploy.ps1` desde VS Code con **Ctrl+Alt+D**, o en PowerShell desde `C:\ForjaEvento` escribe `powershell -ExecutionPolicy Bypass -File .\deploy.ps1`.
4. Responde a la pregunta **«¿Hay un evento en curso? (s/N)»**. Si hay evento, responde `s`; el despliegue se cancela. Si no lo hay, presiona Enter o escribe `n`.
5. El script confirma que la sesión `mc` y el proceso Java de Minecraft estén activos, guarda copias fechadas de los destinos remotos, los reemplaza, envía `reload` y después `kubejs reload server_scripts` a la consola, comprueba que el servidor siga activo y muestra las líneas `ERROR` recientes de ambos logs. Si Minecraft está detenido, cancela antes de modificar archivos.

Opciones:

- `-Config`: agrega `minecraft/kubejs/config`.
- `-Mods`: agrega `minecraft/mods`. **Requiere reiniciar el servidor**; no basta con `reload`.
- `-DryRun`: muestra la lista exacta y no se conecta ni modifica el servidor. Se puede combinar con `-Config` o `-Mods`.

Ejemplos: `powershell -ExecutionPolicy Bypass -File .\deploy.ps1 -DryRun -Config` o `powershell -ExecutionPolicy Bypass -File .\deploy.ps1 -Mods`.

## Si algo falla

- No vuelvas a intentar durante un evento en curso.
- Ejecuta `powershell -ExecutionPolicy Bypass -File .\logs.ps1` para ver solo las últimas líneas con `ERROR` de `logs/latest.log` y `logs/kubejs/server.log`.
- Verifica que el servidor esté arriba y que la sesión tmux se llame `mc`. Un despliegue requiere `ssh.exe`, `scp.exe`, `tar.exe` y acceso con la llave configurada.
- Para scripts KubeJS, el comando documentado es `/kubejs reload server_scripts`; en la consola del servidor se envía sin la barra inicial. `VERIFICAR`: revisa el resultado en el log después de recargar.

## Rollback

Desde `C:\ForjaEvento`, ejecuta `powershell -ExecutionPolicy Bypass -File .\rollback.ps1`. Confirma que no haya un evento en curso. Restaura el último respaldo fechado, guarda una copia de seguridad del estado actual y envía `reload` a tmux. Los respaldos están en `/home/ubuntu/servidor/backups/forja/`.

## Cambiar la IP

Edita `deploy.config.json` en la raíz `C:\ForjaEvento` y cambia el valor `host`. No edites `deploy.ps1`. La llave nunca se copia ni se imprime. En la primera conexión con una IP nueva, OpenSSH acepta y registra su clave de host mediante `StrictHostKeyChecking=accept-new`; confirma que la IP realmente corresponde a tu servidor antes de desplegar.

## Atajo de VS Code

La tarea **Desplegar a AWS** está en `.vscode/tasks.json` y `Ctrl+Alt+D` quedó configurado en el VS Code de esta computadora. El archivo `.vscode/keybindings.json` conserva el mismo atajo como referencia para instalarlo en otra computadora; VS Code no activa atajos del proyecto automáticamente. También puedes ejecutar la tarea con `Ctrl+Shift+P` > **Tasks: Run Task** > `Desplegar a AWS`.

## Errores legacy conocidos

En la recarga final KubeJS informó `7/7` scripts, `0 errores` y `0 advertencias`. `logs/latest.log` mostró seis tablas de botín y dos funciones legacy que dependen de ítems de mods retirados: `torneo:tienda/herramientas/comprar_guantes` y `torneo:tienda/comprar_netherite_gold`. Están pendientes de la limpieza de la Fase 6.
