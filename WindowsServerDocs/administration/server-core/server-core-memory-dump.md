---
title: Configurar los archivos de volcado de memoria para la instalación Server Core
description: Obtenga información sobre cómo configurar los archivos de volcado de memoria para una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: bd22378ec7ce5a1ff4e39546246e6e85ca859c45
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1448203"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurar los archivos de volcado de memoria para la instalación Server Core

>Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Use los siguientes pasos para configurar un volcado de memoria para la instalación Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Paso 1: Deshabilitar la administración de archivos de página automática del sistema

El primer paso consiste en configurar manualmente las opciones de recuperación y errores del sistema. Este proceso es necesario para completar los pasos restantes.

Ejecuta el siguiente comando: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Paso 2: Configurar la ruta de acceso de destino para un volcado de memoria

No es necesario que el archivo de página en la partición donde está instalado el sistema operativo. Para colocar el archivo de paginación en otra partición, debe crear una nueva entrada del registro denominada **DedicatedDumpFile**. Puede definir el tamaño del archivo de paginación mediante la entrada del registro **DumpFileSize** . Para crear las entradas del registro DedicatedDumpFile y DumpFileSize, siga estos pasos: 

1. En el símbolo del sistema, ejecute el comando **regedit** para abrir el Editor del registro.
2. Busque y, a continuación, haga clic en la siguiente subclave del registro: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Haga clic en **Editar > Nuevo > valor de cadena**.
4. Nombre del nuevo valor de **DedicatedDumpFile**y, a continuación, presione ENTRAR.
5. Haga clic en **DedicatedDumpFile**y, a continuación, haga clic en **Modificar**.
6. En tipo de **datos del valor** **\ < unidad > compartida\: \\\ < Dedicateddumpfile.sys\ >** y, a continuación, haga clic en **Aceptar**.

   >[!NOTE] 
   > Reemplazar \ < unidad > compartida\ con una unidad que tenga suficiente espacio para el archivo de paginación y reemplace \ < Dedicateddumpfile.dmp\ > con la ruta de acceso completa al archivo dedicado.
 
7. Haga clic en **Editar > Nuevo > valor DWORD**.
8. Escriba **DumpFileSize**y, a continuación, presione ENTRAR.
9. Haga clic en **DumpFileSize**y, a continuación, haga clic en **Modificar**.
10. En **Editar valor DWORD**, en **Base**, haga clic en **Decimal**.
11. En los **datos del valor**, escriba el valor adecuado y, a continuación, haga clic en **Aceptar**.
   >[!NOTE]
   > El tamaño del archivo de volcado es en megabytes (MB).
12. Salga del Editor del registro.

Después de determinar la ubicación de la partición del volcado de memoria, configure la ruta de acceso de destino para el archivo de página. Para ver la ruta de acceso de destino actual para el archivo de página, ejecute el siguiente comando:

```
wmic RECOVEROS get DebugFilePath
```

El destino predeterminado de **DebugFilePath** es % systemroot%\memory.dmp. Para cambiar la ruta de acceso de destino actual, ejecute el siguiente comando:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Establecer \ < FilePath\ > a la ruta de acceso de destino. Por ejemplo, el siguiente comando establece la ruta de acceso de destino de volcado de memoria en C:\WINDOWS\MEMORY. DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Paso 3: Establecer el tipo de volcado de memoria

Determinar el tipo de volcado de memoria para configurar el servidor. Para ver el tipo de volcado de memoria actual, ejecute el siguiente comando:

```
wmic RECOVEROS get DebugInfoType
```

Para cambiar el tipo de volcado de memoria actual, ejecute el siguiente comando: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\ < Value\ > puede ser 0, 1, 2 o 3, tal como se define por debajo.

- 0: deshabilitar la eliminación de un volcado de memoria.
- 1: volcado de memoria completa. Registra todo el contenido de la memoria del sistema cuando el equipo se detiene inesperadamente. Un volcado de memoria completa puede contener datos de procesos que se estaban ejecutando cuando se recopiló el volcado de memoria.
- 2: volcado de memoria kernel (valor predeterminado). Registra únicamente en la memoria de kernel. Esto acelera el proceso de grabar información en un archivo de registro cuando el equipo se detiene inesperadamente.
- 3: volcado de memoria pequeña. Registra el conjunto más pequeño de información útil que puede ayudar a identificar la razón por la que el equipo se ha detenido inesperadamente.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Paso 4: Configurar el servidor se reinicie automáticamente después de generar un volcado de memoria

De forma predeterminada, el servidor se reinicia automáticamente después de que genere un volcado de memoria. Para ver la configuración actual, ejecute el siguiente comando:

```
wmic RECOVEROS get AutoReboot
```

Si el valor de **AutoReboot** es TRUE, el servidor se reiniciará automáticamente después de generar un volcado de memoria. No es necesario realizar ninguna configuración y puede continuar con el paso siguiente.

Si el valor de **AutoReboot** es FALSE, el servidor no se reiniciará automáticamente. Ejecute el siguiente comando para cambiar el valor:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Paso 5: Configurar el servidor para sobrescribir el archivo existente de volcado de memoria

De forma predeterminada, el servidor sobrescribe el archivo de volcado de memoria existentes cuando se crea uno nuevo. Para determinar si los archivos de volcado de memoria existente ya están configurados para sobrescribirá, ejecute el siguiente comando:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Si el valor es 1, el servidor sobrescribirá el archivo de volcado de memoria existente. No es necesario realizar ninguna configuración, y puede continuar con el paso siguiente.

Si el valor es 0, el servidor no sobrescribe el archivo de volcado de memoria existente. Ejecute el siguiente comando para cambiar el valor: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Paso 6: Establecer una alerta administrativa

Determinar si una alerta administrativa es apropiada y establezca **SendAdminAlert** en consecuencia. Para ver el valor actual de SendAdminAlert, ejecute el siguiente comando:

```
wmic RECOVEROS get SendAdminAlert
```

Los valores posibles para SendAdminAlert son TRUE o FALSE. Para modificar el valor de SendAdminAlert existente en true, ejecute el siguiente comando: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Paso 7: Establecer el tamaño del archivo de página de volcado de memoria

Para comprobar la configuración del archivo de página actual, ejecute uno de los siguientes comandos:

   ```
   wmic.exe pagefile
   ```

   o bien

   ```
   wmic.exe pagefile list /format:list
   ```

Por ejemplo, ejecute el siguiente comando para configurar los tamaños iniciales y máximos de archivo de su página:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Paso 8: Configurar el servidor para generar un volcado de memoria manual

Puede generar manualmente un volcado de memoria mediante el uso de un teclado PS/2. Esta característica está deshabilitada de forma predeterminada, y no está disponible para los teclados de Bus serie Universal (USB).

Para habilitar el manual de la memoria vuelca mediante el uso de un teclado PS/2, ejecute el siguiente comando:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Para determinar si la característica se ha habilitado correctamente, ejecute el siguiente comando:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Debe reiniciar el servidor para que los cambios surtan efecto. Puede reiniciar el servidor, ejecute el comando siguiente:

```
Shutdown / r / t 0
```

Puede generar volcados de memoria manual con un teclado PS/2 que está conectado a su servidor mientras mantiene presionada la tecla CTRL derecha y presionando la tecla Bloq Despl dos veces. Esto hace que el equipo de comprobación con código de error 0xE2 de error. Después de reiniciar el servidor, aparece un nuevo archivo de volcado en la ruta de acceso de destino que estableció en el paso 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Paso 9: Compruebe que se van a crear correctamente los archivos de volcado de memoria

Puede usar el utilidad dumpchk.exe para comprobar que los archivos de volcado de memoria se van a crear correctamente. La utilidad de dumpchk.exe no está instalada con la opción de instalación Server Core, por lo que tendrá que ejecutarlo desde un servidor con la experiencia de escritorio o desde el 10 de Windows. Además, deben estar instaladas las herramientas de depuración para los productos de Windows.  

La utilidad de dumpchk.exe le permite transferir el archivo de volcado de memoria de la instalación Server Core de Windows Server 2008 al otro equipo utilizando el medio de su elección.

> [!WARNING]
> Archivos de página pueden ser muy grandes, por lo que se debe considerar detenidamente la transferencia del método y los recursos que requiere el método.
 

Referencias adicionales

Para obtener información general sobre el uso de archivos de volcado de memoria, vea [información general de las opciones del archivo de volcado de memoria para Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Para obtener más información acerca de los archivos de volcado dedicado, vea [cómo usar el valor del registro DedicatedDeumpFile para superar las limitaciones de espacio en la unidad del sistema al capturar un volcado de memoria del sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



