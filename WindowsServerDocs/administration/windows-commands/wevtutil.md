---
title: wevtutil
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 55a76d58ba7a473881dade55c4f00052c9764ae9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192701"
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
|{gl \| get-log} \<Logname> [/f:\<Format>]|Muestra información de configuración para el registro especificado, que incluye si el registro está habilitado o no, el límite de tamaño máximo actual del registro y la ruta de acceso al archivo donde se almacena el registro.|
|{sl \| conjunto registro} \<nombreDeRegistro > [/ e:\<habilitado >] [/ i:\<aislamiento >] [/ lfn:\<Logpath >] [/ rt:\<retención >] [/ ab:\<automática >] [/ ms:\< MaxSize >] [/ l:\<nivel >] [/ k:\<palabras clave >] [/ ca:\<canal >] [/ c:\<Config >]|Modifica la configuración del registro especificada.|
|{ep \| enum publicadores}|Muestra los publicadores de eventos en el equipo local.|
|{gp \| get-publisher} \<Publishername > [/ ge:\<metadatos >] [/ gm:\<mensaje >] [/ f:\<formato >]]|Muestra la información de configuración para el publicador de eventos especificado.|
|{im \| install-manifest} \<manifiesto >|Instala registros y publicadores de eventos de un manifiesto. Para obtener más información acerca de los manifiestos de eventos y el uso de este parámetro, consulte el SDK de registro de eventos de Windows en el sitio Web de Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{um \| manifiesto desinstalar} \<manifiesto >|Desinstala todos los publicadores y los registros de un manifiesto. Para obtener más información acerca de los manifiestos de eventos y el uso de este parámetro, consulte el SDK de registro de eventos de Windows en el sitio Web de Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|{qe \| eventos de consulta} \<ruta de acceso > [/ lf:\<ArchivoDeRegistro >] [/ sq:\<Structquery >] [/ p:\<consulta >] [/ bm:\<marcador >] [/ sbm:\<Savebm >] [/ rd:\< Dirección >] [/ f:\<formato >] [/ l:\<configuración regional >] [/ c:\<recuento >] [/ e:\<elemento >]|Lee los eventos de un registro de eventos desde un archivo de registro, o mediante una consulta estructurada. De forma predeterminada, proporcione un nombre de registro para \<ruta de acceso >. Sin embargo, si usa el **/lf** opción, a continuación, \<ruta de acceso > debe ser una ruta de acceso a un archivo de registro. Si usas el **/sq** parámetro, \<ruta de acceso > debe ser una ruta de acceso a un archivo que contiene una consulta estructurada.|
|{gli \| get-loginfo} \<Logname> [/lf:\<Logfile>]|Muestra información de estado sobre un registro de eventos o el archivo de registro. Si el **/lf** se utiliza la opción, \<nombreDeRegistro > es una ruta de acceso a un archivo de registro. Puede ejecutar **wevtutil el** para obtener una lista de nombres de registro.|
|{epl \| export-log} \<Path> \<Exportfile> [/lf:\<Logfile>] [/sq:\<Structquery>] [/q:\<Query>] [/ow:\<Overwrite>]|Exporta los eventos de un registro de eventos desde un archivo de registro, o mediante una consulta estructurada en el archivo especificado. De forma predeterminada, proporcione un nombre de registro para \<ruta de acceso >. Sin embargo, si usa el **/lf** opción, a continuación, \<ruta de acceso > debe ser una ruta de acceso a un archivo de registro. Si usas el **/sq** opción \<ruta de acceso > debe ser una ruta de acceso a un archivo que contiene una consulta estructurada. \<Exportfile > es una ruta de acceso al archivo donde se almacenarán los eventos exportados.|
|{al \| archive-log} \<Logpath > [/ l:\<configuración regional >]|Archiva el archivo de registro especificado en un formato independiente. Se crea un subdirectorio con el nombre de la configuración regional y se guarda toda la información específica de la configuración regional en ese subdirectorio. Después de que el directorio y archivo de registro se crean ejecutando **wevtutil al**, se pueden leer eventos en el archivo si el publicador está instalado o no.|
|{cl \| clear-log} \<nombreDeRegistro > [/ bu:\<copia de seguridad >]|Borra los eventos del registro de eventos especificado. El **/Bu** opción puede utilizarse para realizar copias de seguridad de los eventos borrados.|

## <a name="options"></a>Opciones

|Opción|Descripción|
|------|-----------|
|/ f:\<formato >|Especifica que la salida debe ser el formato XML o texto. Si \<formato > es XML, se muestra el resultado en formato XML. Si \<formato > es texto, se muestra el resultado sin etiquetas XML. El valor predeterminado es texto.|
|/ e:\<habilitado >|Habilita o deshabilita un registro. \<Habilitado > puede ser true o false.|
|/ i:\<aislamiento >|Establece el modo de aislamiento de registro. \<Aislamiento > puede ser la aplicación del sistema, o personalizado. El modo de aislamiento de un registro determina si un registro de recursos compartidos de una sesión con otros registros en la misma clase de aislamiento. Si especifica el aislamiento del sistema, el registro de destino comparten al menos permisos con el registro del sistema de escritura. Si especifica el aislamiento de aplicaciones, el registro de destino comparten al menos permisos con el registro de aplicaciones de escritura. Si especifica aislamiento personalizado, también debe proporcionar un descriptor de seguridad mediante la **/ca** opción.|
|/lfn:\<Logpath>|Define el nombre de archivo de registro. \<LogPath > es una ruta de acceso completa al archivo donde el servicio de registro de eventos almacena eventos para este registro.|
|/RT:\<retención >|Establece el modo de retención de registro. \<Retención > puede ser true o false. El modo de retención de registro determina el comportamiento del servicio de registro de eventos cuando un registro alcanza su tamaño máximo. Si un registro de eventos alcanza su tamaño máximo y el modo de retención de registro es true, los eventos existentes se conservan y se descartan los eventos de entrada. Si el modo de retención de registro es false, los eventos entrantes sobrescriben los eventos más antiguos en el registro.|
|/ AB:\<automática >|Especifica la directiva de copia de seguridad automática del registro. \<Auto > puede ser true o false. Si este valor es true, el registro se copiarán automáticamente cuando alcanza el tamaño máximo. Si este valor es true, el período de retención (especificado con el **/rt** opción) también debe establecerse en true.|
|/ms:\<MaxSize>|Establece el tamaño máximo del registro en bytes. El tamaño del registro mínimo es 1048576 bytes (1024KB) y los archivos de registro siempre son múltiplos de 64KB, por lo que el valor que escribe se redondeará según corresponda.|
|/ l:\<nivel >|Define el filtro de nivel del registro. \<Nivel > puede ser cualquier valor de nivel válido. Esta opción solo es aplicable a los registros con una sesión dedicada. Puede quitar un filtro de nivel estableciendo <Level> en 0.|
|/ k:\<palabras clave >|Especifica el filtro de palabras clave del registro. \<Palabras clave > puede ser cualquier máscara de palabra clave válida de 64 bits. Esta opción solo es aplicable a los registros con una sesión dedicada.|
|/ca:\<Channel>|Establece el permiso de acceso para un registro de eventos. \<Canal > es un descriptor de seguridad que utiliza el lenguaje de definición de descriptores de seguridad (SDDL). Para obtener más información acerca del formato SDDL, consulte el sitio Web de Microsoft Developers Network (MSDN) ([https://msdn.microsoft.com](https://msdn.microsoft.com)).|
|/ c:\<Config >|Especifica la ruta de acceso a un archivo de configuración. Esta opción hará que las propiedades de registro para leerse desde el archivo de configuración definido en \<Config >. Si utiliza esta opción, no debe especificar un <Logname> parámetro. El nombre del registro se leerá desde el archivo de configuración.|
|/GE:\<metadatos >|Obtiene información de metadatos para los eventos que pueden ser generados por este publicador. \<Metadatos > puede ser true o false.|
|/GM:\<mensaje >|Muestra el mensaje real en lugar del identificador numérico mensaje. \<Mensaje > puede ser true o false.|
|/lf:\<Logfile>|Especifica que se deben leer los eventos de un registro o desde un archivo de registro. \<Archivo de registro > puede ser true o false. Si es true, el parámetro para el comando es la ruta de acceso a un archivo de registro.|
|/ sq:\<Structquery >|Especifica que se deben obtener los eventos con una consulta estructurada. \<Structquery > puede ser true o false. Si es true, <Path> es la ruta de acceso a un archivo que contiene una consulta estructurada.|
|/ q:\<consulta >|Define la consulta XPath para filtrar los eventos que se leen o se exporta. Si no se especifica esta opción, se devuelven todos los eventos o exportados. Esta opción no está disponible cuando **/sq** es true.|
|/BM:\<marcador >|Especifica la ruta de acceso a un archivo que contiene un marcador de una consulta anterior.|
|/SBM:\<Savebm >|Especifica la ruta de acceso a un archivo que se usa para guardar un marcador de esta consulta. La extensión de nombre de archivo debe ser. Xml.|
|/RD:\<dirección >|Especifica la dirección en la que se leen los eventos. \<Dirección > puede ser true o false. Si es true, se devuelven en primer lugar los eventos más recientes.|
|/ l:\<locale >|Define una cadena de configuración regional que se usa para imprimir el texto del evento en una configuración regional específica. Solo está disponible cuando se imprime los eventos en formato de texto con el **/f** opción.|
|/ c:\<recuento >|Establece el número máximo de eventos para leer.|
|/ e:\<elemento >|Incluye un elemento raíz al mostrar los eventos en XML. \<Elemento > es la cadena que desee dentro del elemento raíz. Por ejemplo, **/e:root** daría lugar a XML que contiene el par de elementos raíz \<raíz ></root>.|
|/ ow:\<sobrescritura >|Especifica que se debe sobrescribir el archivo de exportación. \<Sobrescribir > puede ser true o false. Si especifican true y el archivo de exportación en <Exportfile> ya existe, se sobrescribirá sin pedir confirmación.|
|/ Bu:\<copia de seguridad >|Especifica la ruta de acceso a un archivo donde se almacenarán los eventos borrados. Incluya la extensión. evtx el nombre del archivo de copia de seguridad.|
|/ r:\<remoto >|Ejecuta el comando en un equipo remoto. \<Remoto > es el nombre del equipo remoto. El **im** y **um** no admiten parámetros de operación remota.|
|/ u:\<nombre de usuario >|Especifica un usuario diferente para iniciar sesión en un equipo remoto. \<Nombre de usuario > es un nombre de usuario en el formato dominio\usuario o el usuario. Esta opción solo es aplicable cuando la **/r** se especifica la opción.|
|/ p:\<contraseña >|Especifica la contraseña del usuario. Si el **/u** se usa la opción y no se especifica esta opción o \<contraseña > es "*", se pedirá al usuario que escriba una contraseña. Esta opción solo es aplicable cuando la **/u** se especifica la opción.|
|/ a:\<Auth >|Define el tipo de autenticación para conectarse a un equipo remoto. \<Autenticación > puede ser el predeterminado, Negotiate, Kerberos o NTLM. El valor predeterminado es Negotiate.|
|/uni:\<Unicode>|Muestra el resultado en formato Unicode. \<Unicode > puede ser true o false. Si <Unicode> es true, el resultado es en formato Unicode.|

## <a name="remarks"></a>Comentarios

-   Uso de un archivo de configuración con el parámetro sl

    El archivo de configuración es un archivo XML con el mismo formato que la salida de wevtutil gl \<nombreDeRegistro > /f:xml. El ejemplo siguiente muestra el formato de un archivo de configuración que habilita la retención, habilita la copia de seguridad automatizada y establece el tamaño máximo del registro en el registro de aplicación:  
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

## <a name="BKMK_examples"></a>Ejemplos

Enumerar los nombres de todos los registros:
```
wevtutil el
```
Mostrar información de configuración sobre el registro del sistema en el equipo local en formato XML:
```
wevtutil gl System /f:xml
```
Use un archivo de configuración para establecer los atributos de registro de eventos (vea la sección Comentarios para obtener un ejemplo de un archivo de configuración):
```
wevtutil sl /c:config.xml
```
Mostrar información sobre el publicador de eventos de Microsoft-Windows-Eventlog, incluidos los metadatos acerca de los eventos que puede provocar el publicador:
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Instalar los publicadores y los registros desde el archivo de manifiesto miManifiesto.XML:
```
wevtutil im myManifest.xml
```
Desinstalar los publicadores y los registros del archivo de manifiesto miManifiesto.XML:
```
wevtutil um myManifest.xml
```
Mostrar los tres eventos más recientes del registro de aplicación en formato de texto:
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Mostrar el estado del registro de aplicación:
```
wevtutil gli Application 
```
Exportar los eventos de registro del sistema a C:\backup\system0506.evtx:
```
wevtutil epl System C:\backup\system0506.evtx
```
Borrar todos los eventos del registro de aplicación después de guardarlas en C:\admin\backups\a10306.evtx:
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
