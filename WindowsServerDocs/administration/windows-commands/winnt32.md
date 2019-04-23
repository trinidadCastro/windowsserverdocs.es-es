---
title: winnt32
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a0a6fb3-ba4e-4ace-8984-7f6d3875560e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28675ac6d5a1f1a7f56a9b72ef11a3e99e4ed130
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851266"
---
# <a name="winnt32"></a>winnt32

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Realiza una instalación o actualización de un producto de Windows Server 2003. Puede ejecutar **winnt32** en el símbolo del sistema en un equipo que ejecuta Windows 95, Windows 98, Windows Millennium edition, Windows NT, Windows 2000, Windows XP o un producto en el equipo con Windows Server 2003. Si ejecuta **winnt32** en un equipo con Windows NT versión 4.0, primero debe aplicar el Service Pack 5 o posterior.
## <a name="syntax"></a>Sintaxis
```
winnt32 [/checkupgradeonly] [/cmd: <CommandLine>] [/cmdcons] [/copydir:{i386|ia64}\<FolderName>] [/copysource: <FolderName>] [/debug[<Level>]:[ <FileName>]] [/dudisable] [/duprepare: <pathName>] [/dushare: <pathName>] [/emsport:{com1|com2|usebiossettings|off}] [/emsbaudrate: <BaudRate>] [/m: <FolderName>]  [/makelocalsource] [/noreboot] [/s: <Sourcepath>] [/syspart: <DriveLetter>] [/tempdrive: <DriveLetter>] [/udf: <ID>[,<UDB_File>]] [/unattend[<Num>]:[ <AnswerFile>]]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/checkupgradeonly|Comprueba que el equipo para la compatibilidad de actualización con los productos de Windows Server 2003.<br /><br />Si usa esta opción con **/ unattend**, no se requiere ninguna acción del usuario.  En caso contrario, los resultados se muestran en la pantalla y guardarlos en el nombre de archivo que especifique. El nombre de archivo predeterminado es **upgrade.txt** en la carpeta raíz.|
|/cmd|Indica al programa de instalación para llevar a cabo un comando específico antes de la fase final de la instalación. Esto se produce después de haber reiniciado el equipo y el programa de instalación ha recopilado la información de configuración necesaria, pero antes de completada la instalación.|
|\<CommandLine>|Especifica la línea de comandos que se lleven a cabo antes de la fase final de la instalación.|
|/cmdcons|En un equipo x86, instala la consola de recuperación como una opción de inicio.  La consola de recuperación es una interfaz de línea de comandos desde el que se pueden realizar tareas como iniciar y detener servicios y tener acceso a la unidad local (incluidas las unidades con formato NTFS). Sólo se puede utilizar el **/cmdcons** opción una vez finalizada la instalación.|
|/copydir|crea una carpeta adicional dentro de la carpeta donde se instalan los archivos del sistema operativo.  Por ejemplo, para x86 y equipos basados en x64 64, podría crear una carpeta denominada *controladoresPrivados* dentro de la carpeta de origen para la instalación y ubicar los archivos de controlador en la carpeta i386. tipo **/copydir: i386\\*** controladoresPrivados* para que el programa de instalación copie esa carpeta en el equipo recién instalado, hacer que la nueva ubicación de carpeta **systemroot** \\*ControladoresPrivados *.<br /><br />-   **i386** especifica i386<br />-   **ia64** especifica ia64<br /><br />Puede usar **/copydir** para crear tantas carpetas adicionales como desee.|
|\<FolderName>|Especifica la carpeta que ha creado para guardar las modificaciones de su sitio.|
|/ copysource|crea una carpeta temporal adicional dentro de la carpeta donde se instalan los archivos del sistema operativo. Puede usar **/copysource** para crear tantas carpetas adicionales como desee.<br /><br />A diferencia de las carpetas **/copydir** crea, **/copysource** carpetas se eliminan tras finalizar la instalación.|
|/debug|crea un registro de depuración en el nivel especificado, por ejemplo, **debug4**.  El archivo de registro predeterminado es **C:\ systemroot\winnt32.log**, y|
|\<level>|Nivel de valores y descripciones<br /><br />-   0: Errores graves<br />-   1: Errores<br />-   2: Nivel predeterminado. Warnings<br />-   3: Información de<br />-4: información detallada para la depuración<br /><br />Cada nivel incluye los niveles inferiores.|
|/dudisable|Impide la ejecución de actualización dinámica. Sin la actualización dinámica, el programa de instalación ejecuta solo los archivos de instalación original. Esta opción deshabilita la actualización dinámica incluso si utiliza un archivo de respuesta y especifica las opciones de actualización dinámica en ese archivo.|
|/duprepare|Lleva a cabo las preparaciones de un recurso compartido de instalación para que se puede usar con archivos de actualización dinámica que descargó desde el sitio Web de Windows Update. Este recurso compartido, a continuación, puede utilizarse para la instalación de Windows XP para varios clientes.|
|\<pathName>|Especifica el nombre de ruta de acceso completa.|
|/dushare|Especifica un recurso compartido en el que descargó anteriormente archivos de actualización dinámica (archivos actualizados para su uso con el programa de instalación) desde el sitio Web de Windows Update y en el que ejecutó anteriormente **/duprepare: *** < pathName >*. Cuando se ejecuta en un cliente, especifica que la instalación de cliente usará los archivos actualizados en el recurso compartido especificado en <pathName>.|
|/emsport|Habilita o deshabilita los servicios de administración de emergencia durante la instalación y una vez instalado el sistema operativo de servidor. Con servicios de administración de emergencia, puede administrar de forma remota un servidor en situaciones de emergencia que normalmente requeriría un teclado local, el mouse y el monitor, por ejemplo, cuando la red no está disponible o el servidor no está funcionando correctamente. Servicios de administración de emergencia tiene requisitos de hardware específicos y solo está disponible para los productos de Windows Server 2003.<br /><br />-   **COM1** solo es aplicable para equipos x86 (no en equipos basados en arquitectura Itanium).<br />-   **COM2**solo es aplicable para equipos x86 (no en equipos basados en arquitectura Itanium).<br />-Default. Utiliza la configuración especificada en la tabla de redirección de consola de puerto serie de BIOS (SPCR), o bien, en sistemas basados en arquitectura Itanium, a través de la ruta de acceso de dispositivo de consola EFI. Si especifica **usebiossettings** y no hay ninguna tabla SPCR o ruta de acceso del dispositivo de consola EFI adecuada, no se habilitará Serices de administración de emergencia.<br />-   **desactivar** deshabilita los servicios de administración de emergencia. Puede habilitarlo más adelante modificando la configuración de arranque.|
|/emsbaudrate|para los equipos x86, especifica la velocidad en baudios para los servicios de administración de emergencia. (La opción no es aplicable para equipos basados en arquitectura Itanium). Debe utilizarse con **/emsport: COM1** o **/emsport: COM2** (en caso contrario, **/emsbaudrate** se omite).|
|\<BaudRate>|Especifica la velocidad en baudios de 9600, 19200, 57600 o 115200. 9600 es el valor predeterminado.|
|/m|Especifica que la instalación copie archivos de sustitución de una ubicación alternativa.  Indica al programa que busque primero en la ubicación alternativa y, si los archivos se encuentran, para usarlos en lugar de los archivos de la ubicación predeterminada.|
|/makelocalsource|Indica al programa para copiar todos los archivos de origen de instalación en el disco duro local.  Use **/makelocalsource** cuando se instala desde un cd para proporcionar archivos de instalación cuando el cd no está disponible más adelante en la instalación.|
|/noreboot|Indica al programa que no reinicie el equipo una vez completada la fase de copia de archivos de programa de instalación para que pueda ejecutar otro comando.|
|/s|Especifica la ubicación de origen de los archivos para la instalación. Para copiar archivos simultáneamente desde varios servidores, escriba el **/s:**\<Sourcepath > opción varias veces (hasta un máximo de ocho). Si escribe la opción varias veces, el primer servidor especificado debe estar disponible o se producirá un error en el programa de instalación.|
|\<Sourcepath>|Especifica el nombre de ruta de acceso de origen completo.|
|/syspart|En un equipo x86, especifica que puede copiar los archivos de inicio del programa de instalación en un disco duro, marca el disco como activo y, a continuación, instalar el disco en otro equipo. Al iniciar ese equipo, automáticamente se inicia con la siguiente fase del programa de instalación.<br /><br />Siempre debe utilizar el **/tempdrive** parámetro con el **/Syspart** parámetro.<br /><br />Puede iniciar **winnt32** con el **/Syspart** opción en un equipo x86 ejecutando Windows NT 4.0, Windows 2000, Windows XP o un producto en Windows Server 2003. Si el equipo está ejecutando Windows NT versión 4.0, requiere el Service Pack 5 o posterior. El equipo no puede ejecutar Windows 95, Windows 98 o Windows Millennium edition.|
|\<DriveLetter>|Especifica la letra de unidad.|
|/tempdrive|dirige el programa de instalación que coloque los archivos temporales en la partición especificada.<br /><br />para una instalación nueva, también se instalará el sistema operativo de servidor en la partición especificada.<br /><br />para realizar una actualización, el **/tempdrive** opción afecta a la colocación de los archivos temporales solo; el sistema operativo se actualizará en la partición desde la que se ejecuta **winnt32**.|
|/udf|Indica un identificador (\<ID >) que la instalación se usa para especificar cómo un archivo de base de datos de unicidad (UDB) modifica un archivo de respuesta (consulte la **/ unattend** opción).  El UDB invalida los valores del archivo de respuesta y el identificador determina qué valores del archivo UDB se utilizan. Por ejemplo, **micompañía.udb** invalida la configuración especificada para el identificador usuarioRAS en el archivo Nuestra_compañía.udb. Si no hay ningún \<archivoUDB > se especifica, el programa de instalación solicita al usuario que inserte un disco que contiene el **$Unique$. udb** archivo.|
|\<ID>|Indica un identificador usado para especificar cómo un archivo de base de datos de unicidad (UDB) modifica un archivo de respuesta.|
|\<UDB_file>|Especifica un archivo de base de datos de unicidad (UDB).|
|/unattend|En un equipo x86, actualiza la versión anterior de Windows NT 4.0 Server (con Service Pack 5 o posterior) o Windows 2000 en modo de instalación desatendida. Todas las configuraciones de usuario se toman de la instalación anterior, por lo que no es necesaria ninguna intervención del usuario durante la instalación.|
|\<num>|Especifica el número de segundos entre el momento en que el programa termine de copiar los archivos y cuando reinicia el equipo. Puede usar \<Num > en cualquier equipo con Windows 98, Windows Millennium edition, Windows NT, Windows 2000, Windows XP o un producto en Windows Server 2003. Si el equipo está ejecutando Windows NT versión 4.0, requiere el Service Pack 5 o posterior.|
|\<AnswerFile>|Proporciona el programa de instalación con las especificaciones personalizadas|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
Si va a implementar Windows XP en equipos cliente, puede usar la versión de winnt32.exe que se incluye con Windows XP. Otra manera de implementar Windows XP es utilizar winnt32.msi, que funciona a través de Windows Installer, parte de IntelliMirror conjunto de tecnologías. Para obtener más información acerca de las implementaciones de cliente, consulte el Kit de implementación de Windows Server 2003, que se describe en [mediante la implementación de Windows y los Kits de recursos](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx).

En un equipo basado en Itanium, **winnt32** se puede ejecutar desde la interfaz de Firmware Extensible (EFI) o desde Windows Server 2003 Enterprise, Windows Server 2003 R2 Enterprise, Windows Server 2003 R2 Datacenter o Windows Server 2003 Centro de datos. Además, en un equipo basado en arquitectura Itanium, **/cmdcons** y **/Syspart** no están disponibles y opciones relacionadas con las actualizaciones no están disponibles.
Para obtener más información sobre la compatibilidad de hardware, consulte [compatibilidad de Hardware](https://technet.microsoft.com/library/cc757927(v=ws.10).aspx).
Para obtener más información sobre cómo usar la actualización dinámica e instalar varios clientes, consulte el Kit de implementación de Windows Server 2003, que se describe en [mediante la implementación de Windows y los Kits de recursos](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx).
Para obtener información acerca de cómo modificar la configuración de arranque, consulte la implementación de Windows y los Kits de recursos de Windows Server 2003. Para obtener más información, consulte [mediante la implementación de Windows y los Kits de recursos](https://technet.microsoft.com/library/cc779317(v=ws.10).aspx).
Mediante el **/ unattend** la opción de línea de comandos para automatizar la instalación confirma que ha leído y aceptado el contrato de licencia de Microsoft para Windows Server 2003. Antes de usar esta opción de línea de comandos para instalar Windows Server 2003 en nombre de una organización distinto del suyo propio, debe confirmar que el usuario final (ya sea un individuo o una sola entidad) ha recibido, lea y aceptado los términos de License de Microsoft Acuerdo para ese producto.  Los OEM no pueden especificar esta clave en los equipos vendidos a los usuarios finales.

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
