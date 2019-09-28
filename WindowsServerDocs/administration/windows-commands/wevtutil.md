---
title: wevtutil
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21f3b721a2a08a7fa101ec09f1f11b5e984f0113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362166"
---
# <a name="wevtutil"></a>wevtutil



Permite recuperar información acerca de los registros de eventos y los editores. También puede utilizar este comando para instalar y desinstalar los manifiestos de eventos, ejecutar consultas, y exportar, archivar y borrar registros. Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
wevtutil [{el | enum-logs}] [{gl | get-log} <Logname> [/f:<Format>]]
[{sl | set-log} <Logname> [/e:<Enabled>] [/i:<Isolation>] [/lfn:<Logpath>] [/rt:<Retention>] [/ab:<Auto>] [/ms:<MaxSize>] [/l:<Level>] [/k:<Keywords>] [/ca:<Channel>] [/c:<Config>]] 
[{ep | enum-publishers}] 
[{gp | get-publisher} <Publishername> [/ge:<Metadata>] [/gm:<Message>] [/f:<Format>]] [{im | install-manifest} <Manifest>] 
[{um | uninstall-manifest} <Manifest>] [{qe | query-events} <Path> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/bm:<Bookmark>] [/sbm:<Savebm>] [/rd:<Direction>] [/f:<Format>] [/l:<Locale>] [/c:<Count>] [/e:<Element>]] 
[{gli | get-loginfo} <Logname> [/lf:<Logfile>]] 
[{epl | export-log} <Path> <Exportfile> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/ow:<Overwrite>]] 
[{al | archive-log} <Logpath> [/l:<Locale>]] 
[{cl | clear-log} <Logname> [/bu:<Backup>]] [/r:<Remote>] [/u:<Username>] [/p:<Password>] [/a:<Auth>] [/uni:<Unicode>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|{el \| enum-logs}|Muestra los nombres de todos los registros.|
|{GL \| Get-log} \<Logname > [/f: \<Format >]|Muestra información de configuración del registro especificado, que incluye si el registro está habilitado o no, el límite de tamaño máximo actual del registro y la ruta de acceso al archivo donde se almacena el registro.|
|{SL \| set-log} \<Logname > [/e: \<Enabled >] [/i: \<Isolation >] [/LFN: \<Logpath >] [/RT: \<Retention >] [/AB: \<Auto >] [/ms: \<MaxSize >] [/l: \<Level >] [/k: @no__ t-9Keywords >] [/CA: 0Channel >] [/c: 1Config >]|Modifica la configuración del registro especificado.|
|{EP \| enum-Publishers}|Muestra los publicadores de eventos en el equipo local.|
|{GP \| Get-Publisher} \<Publishername > [/GE: \<Metadata >] [/GM: \<Message >] [/f: \<Format >]]|Muestra la información de configuración para el publicador de eventos especificado.|
|{im \| install-manifest} \<Manifest >|Instala publicadores de eventos y registros desde un manifiesto. Para obtener más información acerca de los manifiestos de eventos y el uso de este parámetro, vea el SDK del registro de eventos de Windows en el sitio web de Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{Um \| uninstall-manifest} \<Manifest >|Desinstala todos los publicadores y registros de un manifiesto. Para obtener más información acerca de los manifiestos de eventos y el uso de este parámetro, vea el SDK del registro de eventos de Windows en el sitio web de Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{qe \| Query-Events} \<Path > [/LF: \<Logfile >] [/SQ: \<Structquery >] [/q: \<Query >] [/BM: \<Bookmark >] [/SBM: \<Savebm >] [/RD: \<Direction >] [/f: \<Format >] [/l: @no_ _ t-9Locale >] [/c: 0Count >] [/e: 1Element >]|Lee eventos de un registro de eventos, de un archivo de registro o mediante una consulta estructurada. De forma predeterminada, se proporciona un nombre de registro para @no__t > 0Path. Sin embargo, si usa la opción **/LF** , @no__t 1Path > debe ser una ruta de acceso a un archivo de registro. Si usa el parámetro **/sq** , el > \<Path debe ser una ruta de acceso a un archivo que contenga una consulta estructurada.|
|{Gli \| Get-loginfo} \<Logname > [/LF: \<Logfile >]|Muestra información de estado sobre un registro de eventos o un archivo de registro. Si se usa la opción **/LF** , @no__t 1Logname > es una ruta de acceso a un archivo de registro. Puede ejecutar **wevtutil** el para obtener una lista de nombres de registro.|
|{EPL \| Export-log} \<Path > \<Exportfile > [/LF: \<Logfile >] [/SQ: \<Structquery >] [/q: \<Query >] [/OW: \<Overwrite >]|Exporta eventos desde un registro de eventos, desde un archivo de registro o mediante una consulta estructurada al archivo especificado. De forma predeterminada, se proporciona un nombre de registro para @no__t > 0Path. Sin embargo, si usa la opción **/LF** , @no__t 1Path > debe ser una ruta de acceso a un archivo de registro. Si usa la opción **/sq** , el > \<Path debe ser una ruta de acceso a un archivo que contenga una consulta estructurada. \<Exportfile > es una ruta de acceso al archivo en el que se almacenarán los eventos exportados.|
|{al \| Archive-Log} \<Logpath > [/l: \<Locale >]|Archiva el archivo de registro especificado en un formato independiente. Se crea un subdirectorio con el nombre de la configuración regional y toda la información específica de la configuración regional se guarda en ese subdirectorio. Después de crear el directorio y el archivo de registro mediante la ejecución de **wevtutil al**, se pueden leer los eventos del archivo si el publicador está instalado o no.|
|{CL \| Clear-log} \<Logname > [/bu: \<Backup >]|Borra los eventos del registro de eventos especificado. La opción **/bu** se puede usar para realizar una copia de seguridad de los eventos borrados.|

## <a name="options"></a>Opciones

|       Opción       |                                                                                                                                                                                                                                                                 Descripción                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f: @no__t 0Format >    |                                                                                                                                                               Especifica que la salida debe ser un formato XML o de texto. Si @no__t > es XML, el resultado se muestra en formato XML. Si @no__t > es texto, el resultado se muestra sin etiquetas XML. El valor predeterminado es Text.                                                                                                                                                                |
|   /e: @no__t 0Enabled >    |                                                                                                                                                                                                                                         Habilita o deshabilita un registro. @no__t 0Enabled > puede ser true o false.                                                                                                                                                                                                                                          |
|  /i: @no__t 0Isolation >   | Establece el modo de aislamiento del registro. \<Isolation > puede ser System, Application o Custom. El modo de aislamiento de un registro determina si un registro comparte una sesión con otros registros en la misma clase de aislamiento. Si especifica el aislamiento del sistema, el registro de destino compartirá al menos permisos de escritura con el registro del sistema. Si especifica el aislamiento de la aplicación, el registro de destino compartirá al menos permisos de escritura con el registro de aplicaciones. Si especifica el aislamiento personalizado, también debe proporcionar un descriptor de seguridad mediante la opción **/CA** . |
|  /LFN: \<Logpath >   |                                                                                                                                                                                                           Define el nombre del archivo de registro. \<Logpath > es una ruta de acceso completa al archivo donde el servicio registro de eventos almacena los eventos para este registro.                                                                                                                                                                                                           |
|  /RT: \<Retention >  |                                                            Establece el modo de retención del registro. @no__t 0Retention > puede ser true o false. El modo de retención de registros Determina el comportamiento del servicio registro de eventos cuando un registro alcanza su tamaño máximo. Si un registro de eventos alcanza su tamaño máximo y el modo de retención del registro es true, se conservan los eventos existentes y se descartan los eventos entrantes. Si el modo de retención del registro es false, los eventos de entrada sobrescriben los eventos más antiguos del registro.                                                             |
|    /AB: \<Auto >     |                                                                                                                                   Especifica la Directiva de copia de seguridad automática de registros. @no__t 0Auto > puede ser true o false. Si este valor es true, se realizará una copia de seguridad del registro automáticamente cuando alcance el tamaño máximo. Si este valor es true, la retención (especificada con la opción **/RT** ) también debe establecerse en true.                                                                                                                                    |
|   /MS: \<MaxSize >   |                                                                                                                                                                        Establece el tamaño máximo del registro en bytes. El tamaño mínimo del registro es 1048576 bytes (1024 KB) y los archivos de registro son siempre múltiplos de 64 KB, por lo que el valor que especifique se redondeará en consecuencia.                                                                                                                                                                         |
|    /l: @no__t 0Level >     |                                                                                                                                                                     Define el filtro de nivel del registro. \<Level > puede ser cualquier valor de nivel válido. Esta opción solo es aplicable a los registros con una sesión dedicada. Puede quitar un filtro de nivel estableciendo <Level> en 0.                                                                                                                                                                      |
|   /k: @no__t 0Keywords >   |                                                                                                                                                                                         Especifica el filtro de palabras clave del registro. \<Keywords > puede ser cualquier máscara de palabra clave de 64 bits válida. Esta opción solo es aplicable a los registros con una sesión dedicada.                                                                                                                                                                                         |
|   /CA: \<Channel >   |                                                                                                                   Establece el permiso de acceso para un registro de eventos. \<Channel > es un descriptor de seguridad que utiliza el lenguaje de definición de descriptores de seguridad (SDDL). Para obtener más información acerca del formato SDDL, vea el sitio web de Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).                                                                                                                    |
|    /c: @no__t 0Config >    |                                                                                                                                  Especifica la ruta de acceso a un archivo de configuración. Esta opción hará que se lean las propiedades de registro del archivo de configuración definido en \<Config >. Si utiliza esta opción, no debe especificar un parámetro <Logname>. El nombre del registro se leerá desde el archivo de configuración.                                                                                                                                   |
|  /GE: @no__t 0Metadata >   |                                                                                                                                                                                                                 Obtiene información de metadatos para los eventos que puede generar este publicador. @no__t 0Metadata > puede ser true o false.                                                                                                                                                                                                                 |
|   /GM: @no__t 0Message >   |                                                                                                                                                                                                                       Muestra el mensaje real en lugar del identificador de mensaje numérico. @no__t 0Message > puede ser true o false.                                                                                                                                                                                                                        |
|   /LF: @no__t 0Logfile >   |                                                                                                                                                                                  Especifica que los eventos se deben leer desde un registro o desde un archivo de registro. @no__t 0Logfile > puede ser true o false. Si es true, el parámetro del comando es la ruta de acceso a un archivo de registro.                                                                                                                                                                                   |
| /SQ: \<Structquery > |                                                                                                                                                                                Especifica que los eventos se deben obtener con una consulta estructurada. @no__t 0Structquery > puede ser true o false. Si es true, <Path> es la ruta de acceso a un archivo que contiene una consulta estructurada.                                                                                                                                                                                |
|    /q: @no__t 0Query >     |                                                                                                                                                                     Define la consulta XPath para filtrar los eventos que se leen o exportan. Si no se especifica esta opción, se devolverán o exportarán todos los eventos. Esta opción no está disponible cuando **/sq** es true.                                                                                                                                                                     |
|  /BM: \<Bookmark >   |                                                                                                                                                                                                                                 Especifica la ruta de acceso a un archivo que contiene un marcador de una consulta anterior.                                                                                                                                                                                                                                 |
|   /SBM: \<Savebm >   |                                                                                                                                                                                                             Especifica la ruta de acceso a un archivo que se usa para guardar un marcador de esta consulta. La extensión de nombre de archivo debe ser. Xml.                                                                                                                                                                                                              |
|  /RD: \<Direction >  |                                                                                                                                                                                                   Especifica la dirección en la que se leen los eventos. @no__t 0Direction > puede ser true o false. Si es true, primero se devuelven los eventos más recientes.                                                                                                                                                                                                   |
|    /l: @no__t 0Locale >    |                                                                                                                                                                                          Define una cadena de configuración regional que se usa para imprimir el texto de un evento en una configuración regional específica. Solo está disponible cuando se imprimen eventos en formato de texto mediante la opción **/f** .                                                                                                                                                                                          |
|    /c: @no__t 0Count >     |                                                                                                                                                                                                                                                  Establece el número máximo de eventos que se van a leer.                                                                                                                                                                                                                                                  |
|   /e: @no__t 0Element >    |                                                                                                                                                           Incluye un elemento raíz al mostrar los eventos en XML. \<Element > es la cadena que desea incluir en el elemento raíz. Por ejemplo, **/e: root** daría como resultado XML que contiene el par de elementos raíz \<root > </root>.                                                                                                                                                            |
|  /OW: @no__t 0Overwrite >  |                                                                                                                                                                 Especifica que se debe sobrescribir el archivo de exportación. @no__t 0Overwrite > puede ser true o false. Si es true, y el archivo de exportación especificado en <Exportfile> ya existe, se sobrescribirá sin confirmación.                                                                                                                                                                 |
|   /BU: \<Backup >    |                                                                                                                                                                                                      Especifica la ruta de acceso a un archivo en el que se almacenarán los eventos borrados. Incluya la extensión. evtx en el nombre del archivo de copia de seguridad.                                                                                                                                                                                                       |
|    /r: @no__t 0Remote >    |                                                                                                                                                                                            Ejecuta el comando en un equipo remoto. \<Remote > es el nombre del equipo remoto. Los parámetros **im** y **Um** no admiten la operación remota.                                                                                                                                                                                            |
|   /u: @no__t 0Username >   |                                                                                                                                                                          Especifica un usuario diferente para iniciar sesión en un equipo remoto. \<Username > es un nombre de usuario con el formato dominio\usuario o usuario. Esta opción solo es aplicable cuando se especifica la opción **/r** .                                                                                                                                                                          |
|   /p: @no__t 0Password >   |                                                                                                                                               Especifica la contraseña del usuario. Si se usa la opción **/u** y no se especifica esta opción o \<Password > es "@no__t 2", se le pedirá al usuario que escriba una contraseña. Esta opción solo es aplicable cuando se especifica la opción \* @ no__t-1/u @ no__t-2 @ no__t-3.                                                                                                                                                |
|     /a: @no__t 0Auth >     |                                                                                                                                                                                             Define el tipo de autenticación para conectarse a un equipo remoto. \<Auth > puede ser default, Negotiate, Kerberos o NTLM. El valor predeterminado es Negotiate.                                                                                                                                                                                              |
|  /UNI: \<Unicode >   |                                                                                                                                                                                                             Muestra la salida en Unicode. @no__t 0Unicode > puede ser true o false. Si <Unicode> es true, el resultado se encuentra en Unicode.                                                                                                                                                                                                             |

## <a name="remarks"></a>Comentarios

-   Usar un archivo de configuración con el parámetro SL

    El archivo de configuración es un archivo XML con el mismo formato que la salida de wevtutil GL \<Logname >/f: XML. En el ejemplo siguiente se muestra el formato de un archivo de configuración que habilita la retención, habilita la copia de seguridad automática y establece el tamaño máximo del registro en el registro de la aplicación:  
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <channel name="Application" isolation="Application"
    xmlns="https://schemas.microsoft.com/win/2004/08/events">
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="BKMK_examples"></a>Example

Enumerar los nombres de todos los registros:
```
wevtutil el
```
Mostrar información de configuración sobre el registro del sistema en el equipo local en formato XML:
```
wevtutil gl System /f:xml
```
Use un archivo de configuración para establecer los atributos del registro de eventos (consulte la sección Comentarios para obtener un ejemplo de un archivo de configuración):
```
wevtutil sl /c:config.xml
```
Mostrar información sobre el publicador de eventos Microsoft-Windows-EventLog, incluidos los metadatos acerca de los eventos que el publicador puede generar:
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Instale los publicadores y registros del archivo de manifiesto de manifest. xml:
```
wevtutil im myManifest.xml
```
Desinstale publicadores y registros del archivo de manifiesto de manifest. xml:
```
wevtutil um myManifest.xml
```
Muestre los tres eventos más recientes del registro de aplicaciones en formato de texto:
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Mostrar el estado del registro de la aplicación:
```
wevtutil gli Application 
```
Exportar eventos del registro del sistema a C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
Borre todos los eventos del registro de aplicaciones después de guardarlos en C:\admin\backups\a10306.evtx:
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
