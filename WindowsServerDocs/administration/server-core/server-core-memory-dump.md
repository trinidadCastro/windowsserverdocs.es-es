---
title: Configurar archivos de volcado de memoria para la instalación Server Core
description: Obtenga información acerca de cómo configurar archivos de volcado de memoria para una instalación Server Core de Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: e3ef6076465bc7d165b58f1205ff8d0cf25014b7
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077782"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurar archivos de volcado de memoria para la instalación Server Core

>Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Siga estos pasos para configurar un volcado de memoria para la instalación Server Core.

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Paso 1: deshabilitar la administración automática de archivos de páginas del sistema

El primer paso consiste en configurar manualmente las opciones de recuperación y los errores del sistema. Esto es necesario para completar los pasos restantes.

Ejecute el siguiente comando:

```
wmic computersystem set AutomaticManagedPagefile=False
```

## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Paso 2: configurar la ruta de acceso de destino para un volcado de memoria

No es necesario tener el archivo de paginación en la partición en la que está instalado el sistema operativo. Para colocar el archivo de paginación en otra partición, debe crear una nueva entrada del registro denominada **DedicatedDumpFile**. Puede definir el tamaño del archivo de paginación mediante la entrada del registro **DumpFileSize** . Para crear las entradas del registro DedicatedDumpFile y DumpFileSize, siga estos pasos:

1. En el símbolo del sistema, ejecute el comando **regedit** para abrir el editor del registro.
2. Busque y haga clic en la siguiente subclave del registro: HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\CrashControl
3. Haga clic en **editar > nuevo > valor de cadena**.
4. Asigne al nuevo valor el nombre **DedicatedDumpFile**y, a continuación, presione Entrar.
5. Haga clic con el botón secundario en **DedicatedDumpFile**y, a continuación, haga clic en **modificar**.
6. En tipo de **datos del valor** ** \<Drive\> \\ \<Dedicateddumpfile.sys\> :**, y, a continuación, haga clic en **Aceptar**.

   >[!NOTE]
   > Reemplace \<Drive\> por una unidad que tenga suficiente espacio en disco para el archivo de paginación y reemplace \<Dedicateddumpfile.dmp\> por la ruta de acceso completa al archivo dedicado.

7. Haga clic en **editar > nuevo valor DWORD >**.
8. Escriba **DumpFileSize**y, a continuación, presione Entrar.
9. Haga clic con el botón secundario en **DumpFileSize**y, a continuación, haga clic en **modificar**.
10. En **Editar valor DWORD**, en **base**, haga clic en **decimal**.
11. En **información del valor**, escriba el valor adecuado y, a continuación, haga clic en **Aceptar**.
    >[!NOTE]
    > El tamaño del archivo de volcado de memoria se encuentra en megabytes (MB).
12. Salga del editor del registro.

Después de determinar la ubicación de la partición del volcado de memoria, configure la ruta de acceso de destino del archivo de paginación. Para ver la ruta de acceso de destino actual del archivo de paginación, ejecute el siguiente comando:

```
wmic RECOVEROS get DebugFilePath
```

El destino predeterminado para **DebugFilePath** es%SystemRoot%\Memory.DMP. Para cambiar la ruta de acceso de destino actual, ejecute el siguiente comando:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Establezca \<FilePath\> en la ruta de acceso de destino. Por ejemplo, el comando siguiente establece la ruta de acceso de destino de volcado de memoria en C:\WINDOWS\MEMORY. DMP

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```

## <a name="step-3-set-the-type-of-memory-dump"></a>Paso 3: establecer el tipo de volcado de memoria

Determine el tipo de volcado de memoria que se va a configurar para el servidor. Para ver el tipo de volcado de memoria actual, ejecute el siguiente comando:

```
wmic RECOVEROS get DebugInfoType
```

Para cambiar el tipo de volcado de memoria actual, ejecute el siguiente comando:

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Value\> puede ser 0, 1, 2 o 3, tal y como se define a continuación.

- 0: deshabilitar la eliminación de un volcado de memoria.
- 1: volcado de memoria completo. Registra todo el contenido de la memoria del sistema cuando el equipo se detiene de forma inesperada. Un volcado de memoria completo puede contener datos de los procesos que se estaban ejecutando cuando se recopiló el volcado de memoria.
- 2: volcado de memoria del kernel (predeterminado). Registra solo la memoria del kernel. Esto acelera el proceso de grabar información en un archivo de registro cuando el equipo se detiene de forma inesperada.
- 3: volcado de memoria pequeño. Registra el conjunto más pequeño de información útil que puede ayudar a identificar por qué el equipo se detuvo de forma inesperada.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Paso 4: configurar el servidor para que se reinicie automáticamente después de generar un volcado de memoria

De forma predeterminada, el servidor se reinicia automáticamente después de generar un volcado de memoria. Para ver la configuración actual, ejecute el siguiente comando:

```
wmic RECOVEROS get AutoReboot
```

Si el valor de **AutoReboot** es true, el servidor se reiniciará automáticamente después de generar un volcado de memoria. No se necesita ninguna configuración y puede continuar con el paso siguiente.

Si el valor de **AutoReboot** es false, el servidor no se reiniciará automáticamente. Ejecute el siguiente comando para cambiar el valor:

```
wmic RECOVEROS set AutoReboot = true
```

## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Paso 5: configurar el servidor para sobrescribir el archivo de volcado de memoria existente

De forma predeterminada, el servidor sobrescribe el archivo de volcado de memoria existente cuando se crea uno nuevo. Para determinar si los archivos de volcado de memoria existentes ya están configurados para sobrescribirse, ejecute el siguiente comando:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Si el valor es 1, el servidor sobrescribirá el archivo de volcado de memoria existente. No se necesita ninguna configuración y puede continuar con el paso siguiente.

Si el valor es 0, el servidor no sobrescribirá el archivo de volcado de memoria existente. Ejecute el siguiente comando para cambiar el valor:

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```

## <a name="step-6-set-an-administrative-alert"></a>Paso 6: establecer una alerta administrativa

Determine si una alerta administrativa es adecuada y establezca **SendAdminAlert** en consecuencia. Para ver el valor actual de SendAdminAlert, ejecute el siguiente comando:

```
wmic RECOVEROS get SendAdminAlert
```

Los valores posibles de SendAdminAlert son TRUE o FALSE. Para modificar el valor de SendAdminAlert existente como true, ejecute el siguiente comando:

```
wmic RECOVEROS set SendAdminAlert = true
```

## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Paso 7: establecer el tamaño del archivo de paginación del volcado de memoria

Para comprobar la configuración del archivo de paginación actual, ejecute uno de los siguientes comandos:

   ```
   wmic.exe pagefile
   ```

   o

   ```
   wmic.exe pagefile list /format:list
   ```

Por ejemplo, ejecute el siguiente comando para configurar los tamaños inicial y máximo del archivo de paginación:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Paso 8: configurar el servidor para generar un volcado de memoria manual

Puede generar manualmente un volcado de memoria mediante un teclado PS/2. Esta característica está deshabilitada de forma predeterminada y no está disponible para los teclados de bus serie universal (USB).

Para habilitar los volcados de memoria manuales mediante un teclado PS/2, ejecute el siguiente comando:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Para determinar si la característica se ha habilitado correctamente, ejecute el siguiente comando:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Debe reiniciar el servidor para que los cambios surtan efecto. Para reiniciar el servidor, ejecute el comando siguiente:

```
Shutdown / r / t 0
```

Puede generar volcados de memoria manuales con un teclado PS/2 conectado al servidor manteniendo presionada la tecla CTRL derecha mientras presiona la tecla Bloq Despl dos veces. Esto hace que la comprobación de errores del equipo sea el código de error 0xE2. Después de reiniciar el servidor, aparece un nuevo archivo de volcado en la ruta de acceso de destino que estableció en el paso 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Paso 9: comprobar que los archivos de volcado de memoria se están creando correctamente

Puede usar el dumpchk.exe utlity para comprobar que los archivos de volcado de memoria se crean correctamente. La utilidad dumpchk.exe no se instala con la opción de instalación Server Core, por lo que tendrá que ejecutarla desde un servidor con la experiencia de escritorio o desde Windows 10. Además, se deben instalar las herramientas de depuración para productos de Windows.

La utilidad dumpchk.exe le permite transferir el archivo de volcado de memoria de la instalación Server Core de Windows Server 2008 al otro equipo usando el medio que prefiera.

> [!WARNING]
> Los archivos de paginación pueden ser muy grandes, por lo que debe considerar detenidamente el método de transferencia y los recursos que requiere el método.


Referencias adicionales

Para obtener información general sobre el uso de archivos de volcado de memoria, consulte [información general sobre las opciones de archivo de volcado de memoria para Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Para obtener más información acerca de los archivos de volcado dedicados, consulte [Cómo usar el valor del registro DedicatedDeumpFile para superar las limitaciones de espacio en la unidad del sistema al capturar un volcado de memoria del sistema](/archive/blogs/ntdebugging/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump).