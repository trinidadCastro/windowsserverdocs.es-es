---
title: Configurar archivos de volcado de memoria para la instalación de Server Core
description: Obtenga información sobre cómo configurar los archivos de volcado de memoria para una instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 235df6f681de51a12f82b9fad019dd2db45fd486
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435552"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurar archivos de volcado de memoria para la instalación de Server Core

>Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Use los pasos siguientes para configurar un volcado de memoria para la instalación de Server Core. 

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Paso 1: Deshabilitar la administración de archivo de página automática del sistema

El primer paso consiste en configurar manualmente las opciones de error y recuperación del sistema. Esto es necesario para completar los pasos restantes.

Ejecute el siguiente comando: 

```
wmic computersystem set AutomaticManagedPagefile=False
```
 
## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Paso 2: Configurar la ruta de acceso de destino para un volcado de memoria

No debe tener el archivo de paginación en la partición donde está instalado el sistema operativo. Para poner el archivo de paginación en otra partición, debe crear una nueva entrada del registro denominada **DedicatedDumpFile**. Puede definir el tamaño del archivo de paginación mediante el **DumpFileSize** entrada del registro. Para crear las entradas del registro DedicatedDumpFile y DumpFileSize, siga estos pasos: 

1. En el símbolo del sistema, ejecute el **regedit** comando para abrir el Editor del registro.
2. Busque y haga clic en la siguiente subclave del Registro: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl
3. Haga clic en **Edición > Nuevo > valor de cadena**.
4. Asigne al nuevo valor **DedicatedDumpFile**, y, a continuación, presione ENTRAR.
5. Haga clic en **DedicatedDumpFile**y, a continuación, haga clic en **modificar**.
6. En **datos del valor** tipo  **\<unidad\>:\\\<Dedicateddumpfile.sys\>** y, a continuación, haga clic en **Aceptar**.

   >[!NOTE] 
   > Reemplace \<unidad\> con una unidad que tenga suficiente espacio para el archivo de paginación y reemplace \<Dedicateddumpfile.dmp\> con la ruta de acceso completa al archivo dedicado.
 
7. Haga clic en **Edición > Nuevo > valor DWORD**.
8. Tipo **DumpFileSize**, y, a continuación, presione ENTRAR.
9. Haga clic en **DumpFileSize**y, a continuación, haga clic en **modificar**.
10. En **Editar valor DWORD**, en **Base**, haga clic en **Decimal**.
11. En **datos del valor**, escriba el valor adecuado y, a continuación, haga clic en **Aceptar**.
    >[!NOTE]
    > Es el tamaño del archivo de volcado de memoria en megabytes (MB).
12. Salga del Editor del registro.

Después de determinar la ubicación de la partición del volcado de memoria, configure la ruta de acceso de destino para el archivo de paginación. Para ver la ruta de acceso de destino actual del archivo de paginación, ejecute el siguiente comando:

```
wmic RECOVEROS get DebugFilePath
```

El destino predeterminado de **DebugFilePath** es % systemroot%\memory.dmp. Para cambiar la ruta de acceso de destino actual, ejecute el siguiente comando:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Establecer \<FilePath\> a la ruta de acceso de destino. Por ejemplo, el siguiente comando establece la ruta de acceso de destino de volcado de memoria en C:\WINDOWS\MEMORY. DMP: 

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```
 
## <a name="step-3-set-the-type-of-memory-dump"></a>Paso 3: Establezca el tipo de volcado de memoria

Determinar el tipo de volcado de memoria a configurar para el servidor. Para ver el tipo de volcado de memoria actual, ejecute el siguiente comando:

```
wmic RECOVEROS get DebugInfoType
```

Para cambiar el tipo de volcado de memoria actual, ejecute el siguiente comando: 

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Valor\> puede ser 0, 1, 2 o 3, tal como se define a continuación.

- 0: Deshabilitar la eliminación de un volcado de memoria.
- 1: Volcado de memoria completo. Registra todo el contenido de la memoria del sistema cuando el equipo se detiene inesperadamente. Un volcado de memoria completo puede contener datos de los procesos que se estaban ejecutando cuando se recopiló el volcado de memoria.
- 2: Volcado de memoria del kernel (valor predeterminado). Registra únicamente en la memoria de kernel. Esto acelera el proceso de registro de información en un archivo de registro cuando el equipo se detiene inesperadamente.
- 3: Volcado de memoria pequeña. Registra el conjunto más pequeño de información útil que puede ayudar a identificar por qué el equipo se ha detenido inesperadamente.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Paso 4: Configurar el servidor se reinicie automáticamente después de generar un volcado de memoria

De forma predeterminada, el servidor se reinicia automáticamente después de que genere un volcado de memoria. Para ver la configuración actual, ejecute el siguiente comando:

```
wmic RECOVEROS get AutoReboot
```

Si el valor de **AutoReboot** es TRUE, el servidor se reiniciará automáticamente después de generar un volcado de memoria. Se necesita ninguna configuración y puede continuar con el paso siguiente.

Si el valor de **AutoReboot** es FALSE, el servidor no se reiniciará automáticamente. Ejecute el siguiente comando para cambiar el valor:

```
wmic RECOVEROS set AutoReboot = true
```
 
## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Paso 5: Configurar el servidor para sobrescribir el archivo de volcado de memoria existente

De forma predeterminada, el servidor sobrescribe el archivo de volcado de memoria existente cuando se crea uno nuevo. Para determinar si los archivos de volcado de memoria existente ya están configurados para sobrescribirse, ejecute el siguiente comando:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Si el valor es 1, el servidor sobrescribirá el archivo de volcado de memoria existente. Se necesita ninguna configuración y puede continuar con el paso siguiente.

Si el valor es 0, el servidor no sobrescribe el archivo de volcado de memoria existente. Ejecute el siguiente comando para cambiar el valor: 

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```
 
## <a name="step-6-set-an-administrative-alert"></a>Paso 6: Establecer una alerta administrativa

Determinar si una alerta administrativa es adecuado y establezca **SendAdminAlert** en consecuencia. Para ver el valor actual de SendAdminAlert, ejecute el siguiente comando:

```
wmic RECOVEROS get SendAdminAlert
```

Los valores posibles para SendAdminAlert son TRUE o FALSE. Para modificar el valor de SendAdminAlert existente en true, ejecute el siguiente comando: 

```
wmic RECOVEROS set SendAdminAlert = true
```
 
## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Paso 7: Establecer el tamaño de archivo de volcado de memoria de página

Para comprobar la configuración del archivo de página actual, ejecute uno de los siguientes comandos:

   ```
   wmic.exe pagefile
   ```

   o bien

   ```
   wmic.exe pagefile list /format:list
   ```

Por ejemplo, ejecute el comando siguiente para configurar los tamaños iniciales y máximo del archivo de página:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Paso 8: Configurar el servidor para generar un volcado de memoria manual

Puede generar manualmente un volcado de memoria mediante un teclado PS/2. Esta característica está deshabilitada de forma predeterminada, y no está disponible para los teclados de Bus serie Universal (USB).

Para habilitar la memoria manual volcados de memoria mediante un teclado PS/2, ejecute el siguiente comando:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Para determinar si se ha habilitado la característica correctamente, ejecute el siguiente comando:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Debe reiniciar el servidor para que los cambios surtan efecto. Puede reiniciar el servidor, ejecute el comando siguiente:

```
Shutdown / r / t 0
```

Puede generar volcados de memoria manual con un teclado PS/2 que está conectado a su servidor, mantenga presionada la tecla CTRL derecha mientras se presiona la tecla Bloq Despl dos veces. Esto hace que el equipo de comprobación con el código de error 0xE2 de error. Después de reiniciar el servidor, aparece un nuevo archivo de volcado de memoria en la ruta de acceso de destino que estableció en el paso 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Paso 9: Compruebe que se crean correctamente los archivos de volcado de memoria

Puede usar la utilidad dumpchk.exe para comprobar que los archivos de volcado de memoria que se va a se creó correctamente. La utilidad dumpchk.exe no está instalada con la opción de instalación Server Core, por lo que tendrá que ejecutarlo desde un servidor con la experiencia de escritorio o Windows 10. Además, las herramientas de depuración para los productos de Windows deben estar instaladas.  

La utilidad dumpchk.exe le permite transferir el archivo de volcado de memoria de la instalación de Server Core de Windows Server 2008 en el otro equipo mediante el medio de su elección.

> [!WARNING]
> Los archivos de paginación pueden ser muy grandes, por lo que debe tener en cuenta a la transferencia de método y los recursos que requiere el método.
 

Referencias adicionales

Para obtener información general sobre el uso de archivos de volcado de memoria, vea [información general del archivo de volcado de memoria de las opciones para Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Para obtener más información acerca de los archivos de volcado de memoria dedicado, vea [cómo usar el valor del registro DedicatedDeumpFile para superar las limitaciones de espacio en la unidad del sistema al capturar un volcado de memoria del sistema](https://blogs.msdn.microsoft.com/ntdebugging/2010/04/02/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump/).



