---
title: fsutil 8dot3name
description: Tema de referencia del comando fsutil 8dot3name, que consulta o cambia la configuración de un comportamiento de nombre corto (nombre 8.3).
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 02977a33c21560fd2078f0f596f312f4ab4bc408
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436060"
---
# <a name="fsutil-8dot3name"></a>fsutil 8dot3name

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta o cambia la configuración de comportamiento de nombre corto (nombre 8.3), que incluye:

- Consultar la configuración actual del comportamiento de nombre corto.

- Examinando la ruta de acceso al directorio especificada para las claves del registro que podrían verse afectadas si se han quitado nombres cortos de la ruta de acceso al directorio especificada.

- Cambiar la configuración que controla el comportamiento de nombre corto. Esta configuración se puede aplicar a un volumen especificado o a la configuración del volumen predeterminado.

- Quitar los nombres cortos de todos los archivos de un directorio.

> [!IMPORTANT]
> Quitar permanentemente los nombres de archivo 8dot3 y no modificar las claves del registro que apuntan a los nombres de archivo 8dot3 pueden provocar errores inesperados en la aplicación, incluida la incapacidad de desinstalar una aplicación. Se recomienda hacer primero una copia de seguridad del directorio o del volumen antes de intentar quitar los nombres de archivo 8.3.

## <a name="syntax"></a>Sintaxis

```
fsutil 8dot3name [query] [<volumepath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <directorypath>
fsutil 8dot3name [set] { <defaultvalue> | <volumepath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <directorypath>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| misma`[<volumepath>]` | Consulta en el sistema de archivos el estado del comportamiento de creación de nombres cortos 8.3.<p>Si no se especifica un *VolumePath* como parámetro, se muestra la configuración predeterminada del comportamiento de creación de 8dot3name para todos los volúmenes. |
| revisar`<directorypath>` | Examina los archivos que se encuentran en el *directoryPath* especificado para las claves del registro que podrían verse afectadas si se han quitado los nombres cortos 8.3 de los nombres de archivo. |
| conjunto`<defaultvalue> | <volumepath>}` | Cambia el comportamiento del sistema de archivos para la creación de nombres 8dot3 en las instancias siguientes:<ul><li>Cuando se especifica *DefaultValue* , la clave del registro, **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**, se establece en el valor *DefaultValue*.<p>*DefaultValue* puede tener los valores siguientes:<ul><li>**0**: habilita la creación de nombres 8dot3 para todos los volúmenes del sistema.</li><li>**1**: deshabilita la creación de nombres 8dot3 para todos los volúmenes del sistema.</li><li>**2**: establece la creación de nombres 8dot3 en cada volumen.</li><li>**3**: deshabilita la creación de nombres 8dot3 para todos los volúmenes excepto el volumen del sistema.</li></ul><li>Cuando se especifica un *VolumePath* , los volúmenes especificados en las propiedades de la marca de disco se establecen para habilitar la creación de nombres 8dot3 para un volumen especificado (**0**) o para deshabilitar la creación de nombres 8dot3 en el volumen especificado (**1**).<p>Debe establecer el comportamiento predeterminado del sistema de archivos para la creación de nombres 8dot3 en el valor **2** para poder habilitar o deshabilitar la creación de nombres 8dot3 para un volumen especificado.</li></ul> |
| reparto`<directorypath>` | Quita los nombres de archivo 8.3 de todos los archivos que se encuentran en el *directoryPath*especificado. El nombre de archivo 8.3 no se quita para ningún archivo en el que la *directoryPath* combinada con el nombre de archivo contenga más de 260 caracteres.<p>Este comando muestra, pero no modifica las claves del registro que apuntan a los archivos que tenían nombres de archivo 8.3 quitados permanentemente. |
| `<volumepath>` | Especifica el nombre de la unidad seguido de dos puntos o del GUID en el formato `volume{GUID}` . |
| /f | Especifica que todos los archivos que se encuentran en el *directoryPath* especificado tienen los nombres de archivo 8.3 quitados, incluso si hay claves del registro que señalan a archivos con el nombre de archivo 8.3. En este caso, la operación quita los nombres de archivo 8.3, pero no modifica las claves del registro que señalan a los archivos que usan los nombres de archivo 8dot3. **ADVERTENCIA:** Se recomienda hacer una copia de seguridad del directorio o del volumen antes de usar el parámetro **/f** , ya que puede provocar errores inesperados en la aplicación, incluida la incapacidad de desinstalar programas. |
| l`[<log file>]` | Especifica un archivo de registro donde se escribe la información.<p>Si no se especifica el parámetro **/l** , toda la información se escribe en el archivo de registro predeterminado: `%temp%\8dot3_removal_log@(GMT YYYY-MM-DD HH-MM-SS)` . log * * |
| /s | Especifica que la operación se debe aplicar a los subdirectorios del *directoryPath*especificado. |
| /t | Especifica que la eliminación de nombres de archivo 8dot3 debe ejecutarse en modo de prueba. Se realizan todas las operaciones excepto la eliminación real de los nombres de archivo 8.3. Puede usar el modo de prueba para detectar las claves del registro que apuntan a los archivos que usan los nombres de archivo 8.3. |
| /v | Especifica que toda la información que se escribe en el archivo de registro también se muestra en la línea de comandos. |

### <a name="examples"></a>Ejemplos

Para consultar el comportamiento Disable 8.3 Name para un volumen de disco que se especifica con el GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, escriba:

```
fsutil 8dot3name query volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

También puede consultar el comportamiento del nombre 8.3 mediante el subcomando **Behavior** .

Para quitar los nombres de archivo 8dot3 en el directorio *D:\mydata* y todos los subdirectorios, mientras escribe la información en el archivo de registro que se especifica como *logfile. log*, escriba:

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil behavior](fsutil-behavior.md)
