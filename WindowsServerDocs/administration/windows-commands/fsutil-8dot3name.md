---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: fsutil 8dot3name
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4e8d67de42342f5a0828df9f57ca6d7e2870586e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873196"
---
# <a name="fsutil-8dot3name"></a>fsutil 8dot3name

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Las consultas o cambia la configuración de comportamiento de nombre corto (nombre 8.3), que incluyen:

-   Consultar la configuración actual para el comportamiento de nombre corto

-   Analizar la ruta de acceso de directorio especificada para las claves del registro que podrían verse afectados si se eliminan los nombres cortos de la ruta de acceso del directorio especificado

-   Cambiar la configuración que controla el comportamiento de nombre corto. Esta configuración se puede aplicar a un volumen especificado o a la configuración predeterminada del volumen.

-   Quitar los nombres cortos para todos los archivos dentro de un directorio

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------------|---------------|
|consultas [<VolumePath>]|Consulta el sistema de archivos para el estado del comportamiento de creación de nombre corto 8.3.<br /><br />Si un *VolumePath* no se especifica como un parámetro, se muestra la configuración del comportamiento predeterminado 8dot3name creación para todos los volúmenes.|
|examen <DirectoryPath>|Examina los archivos que se encuentran en la instancia especificada *DirectoryPath* para claves del registro que podrían verse afectadas si se eliminan nombres cortos 8.3 de los nombres de archivo.|
|set { <DefaultValue> &#124; <VolumePath>}|Cambia el comportamiento del sistema de archivos para la creación de nombres 8.3 en los casos siguientes:<br /><br /><ul><li>Cuando *DefaultValue* se especifica, la clave del registro, **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**, se establece en el *DefaultValue*.<br /><br />    El *DefaultValue* puede tener los siguientes valores:<br /><br /><ul><li>**0**: Permite la creación de nombres 8.3 para todos los volúmenes en el sistema.</li><li>**1**: Deshabilita la creación de nombres 8.3 para todos los volúmenes en el sistema.</li><li>**2**: Creación de nombres 8.3 se establece en por volumen.</li><li>**3**: Deshabilita la creación de nombres 8.3 para todos los volúmenes, excepto el volumen del sistema.</li></ul></li><li>Cuando un *VolumePath* se especifica, se establecen los volúmenes especificados en las propiedades del disco marca 8dot3name para habilitar la creación de nombres 8.3 para un volumen especificado (**0**) o un conjunto para deshabilitar la creación de nombres 8.3 en el Especifica el volumen (**1**).<br /><br />    Debe establecer el comportamiento del sistema de archivos predeterminado para la creación de nombres 8.3 en el valor **2** antes de habilitar o deshabilitar la creación de nombres 8.3 para un volumen especificado.</li></ul>|
|franja <DirectoryPath>|Quita los nombres de archivo 8.3 para todos los archivos que se encuentran en la instancia especificada *DirectoryPath*. No se quita el nombre de archivo 8.3 para todos los archivos donde el *DirectoryPath* combinada con el archivo de nombre contiene más de 260 caracteres.<br /><br />Este comando enumera, pero no modifica las claves del registro que apuntan a los archivos que tenían los nombres de archivo 8.3 quitado de forma permanente.<br /><br />Para obtener más información sobre los efectos de eliminar permanentemente los nombres de archivo 8.3 de archivos, consulte [comentarios](Fsutil-8dot3name.md#BKMK_remarks).|
|<VolumePath>|Especifica el nombre de la unidad seguido de dos puntos o el GUID en formato **volumen {***GUID***}**.|
|/f|Especifica que todos los archivos que se encuentran en la instancia especificada *DirectoryPath* los nombres de archivo 8.3 quitaron incluso si hay claves del registro que apuntan a archivos con el nombre de archivo 8.3. En este caso, la operación quita los nombres de archivo 8.3, pero no modifica las claves del registro que apuntan a los archivos que usan los nombres de archivo 8.3. **Advertencia:** Se recomienda que se realice una de sus directorios o volúmenes antes de usar el **/f** parámetro porque puede dar lugar a errores inesperados de aplicación, incluida la incapacidad para desinstalar programas.|
|/l [<log file>]|Especifica un archivo de registro donde se escribe información.<br /><br />Si el **/l** parámetro no se especifica, toda la información se escribe en el archivo de registro predeterminado:<br /><br />**% temp%\8dot3_removal_log@ (***GMT aaaa-MM-DD HH-MM-SS***) .log**|
|/s|Especifica que la operación se debe aplicar a los subdirectorios del elemento especificado *DirectoryPath*.|
|/t|Especifica que la eliminación de los nombres de archivo 8.3 se debe ejecutar en modo de prueba. Se realizan todas las operaciones excepto la eliminación real de los nombres de archivo 8.3. Puede usar el modo de prueba para detectar qué registro claves apuntan a archivos que usan los nombres de archivo 8.3.|
|/v|Especifica que toda la información que se escribe en el archivo de registro también se muestra en la línea de comandos.|

## <a name="BKMK_remarks"></a>Comentarios

-   Eliminando los nombres de archivo 8.3 de forma permanente y no modifica las claves del registro que señalan a los nombres de archivo 8.3 pueden provocar errores inesperados de aplicación, incluida la imposibilidad de desinstalar una aplicación. Se recomienda que hacer primero una copia de los directorios o volúmenes antes de intentar quitar los nombres de archivo 8.3.

## <a name="BKMK_examples"></a>Ejemplos
Para consultar el comportamiento de deshabilitación 8.3 para un volumen de disco que se especifica con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

También puede consultar el comportamiento de nombre 8.3 utilizando el **comportamiento** subcomando.

Para quitar los nombres de archivo 8.3 en el *D:\MyData* directorio y todos los subdirectorios, al escribir la información en el archivo de registro que se especifica como *mylogfile.log*, tipo:

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[fsutil 8dot3name](Fsutil-8dot3name.md)

[comportamiento de fsutil](Fsutil-behavior.md)


