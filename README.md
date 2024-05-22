# programa_batch
El objetivo de este script es monitorear las conexiones de red activas y alertar al usuario si se detectan conexiones a IPs que no están en una lista de IPs permitidas. :smile:

## Pasos:
1. Definir una lista de IPs permitidas.
2. Obtener la lista de conexiones de red activas.
3. Comparar las conexiones activas con la lista de IPs permitidas.
4. Alertar al usuario si se detecta una conexión a una IP no permitida.

```
@echo off
setlocal enabledelayedexpansion

:: Define la lista de IPs permitidas
set "allowed_ips=192.168.1.1 192.168.1.2 8.8.8.8"

:: Obtiene la lista de conexiones de red activas
echo Obtaining active network connections...
netstat -n > netstat_output.txt

:: Verifica las conexiones activas
echo Checking for unauthorized connections...
set "unauthorized=false"
for /f "tokens=3" %%a in ('findstr /R /C:"^ *TCP" netstat_output.txt') do (
    set "found=false"
    for %%b in (%allowed_ips%) do (
        if %%a equ %%b (
            set "found=true"
            goto :next_ip
        )
    )
    :next_ip
    if "!found!" equ "false" (
        echo Unauthorized connection found: %%a
        set "unauthorized=true"
    )
)

:: Alerta al usuario si se encontraron conexiones no autorizadas
if "!unauthorized!" equ "true" (
    echo WARNING: Unauthorized connections detected!
) else (
    echo All connections are authorized.
)

:: Limpia los archivos temporales
del netstat_output.txt

endlocal
pause
```
## Explicación del Código:
1. Definición de IPs Permitidas: La variable allowed_ips contiene una lista de IPs que son consideradas seguras.
2. Obtención de Conexiones Activas: Se utiliza el comando netstat -n para obtener una lista de todas las conexiones de red activas y se guarda en un archivo temporal netstat_output.txt.
3. Verificación de Conexiones: Se recorre la lista de conexiones activas y se compara cada IP con las IPs permitidas. Si se encuentra una conexión a una IP no permitida, se marca como no autorizada.
4. Alerta al Usuario: Si se detectan conexiones no autorizadas, se muestra una advertencia en la pantalla.
5. Limpieza: Se elimina el archivo temporal netstat_output.txt.

### Ejecución del Script:
- Copia el código anterior y pégalo en un archivo de texto.
- Guarda el archivo con la extensión .bat, por ejemplo, check_connections.bat.
- Ejecuta el script haciendo doble clic sobre el archivo o ejecutándolo desde la línea de comandos.

### Este script es una herramienta básica para la detección de conexiones de red sospechosas y puede ser ampliado y mejorado según tus necesidades :smile:
