---
title: servermanagercmd
description: Artículo de referencia para el comando ServerManagerCmd, que instala y quita roles, servicios de función y características.
ms.topic: reference
ms.assetid: 507c4b87-8e13-4872-8b34-0c7508eecbc1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 4a00294b70728a41cc68ae15d35a8c19fd768cf7
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389163"
---
# <a name="servermanagercmd"></a>servermanagercmd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Instala y quita roles, servicios de función y características. También muestra la lista de todas las funciones, servicios de función y características disponibles, y muestra las que están instaladas en este equipo.

> [!IMPORTANT]
> Este comando, ServerManagerCmd, ha quedado en desuso y no se garantiza que se admita en versiones futuras de Windows. En su lugar, se recomienda usar los cmdlets de Windows PowerShell que están disponibles para Administrador del servidor. Para obtener más información, consulta el tema sobre la [instalación o desinstalación de roles, servicios de rol o características](/administration/server-manager/install-or-uninstall-roles-role-services-or-features).

## <a name="syntax"></a>Sintaxis

```
servermanagercmd -query [[[<drive>:]<path>]<query.xml>] [-logpath [[<drive>:]<path>]<log.txt>]
servermanagercmd -inputpath  [[[<drive>:]<path>]<answer.xml>] [-resultpath <result.xml> [-restart] | -whatif] [-logpath [[<drive>:]<path>]<log.txt>]
servermanagercmd -install <id> [-allSubFeatures] [-resultpath [[<drive>:]<path>]<result.xml> [-restart] | -whatif] [-logpath [[<Drive>:]<path>]<log.txt>]
servermanagercmd -remove <id> [-resultpath <result.xml> [-restart] | -whatif] [-logpath  [[<drive>:]<path>]<log.txt>]
servermanagercmd [-help | -?]
servermanagercmd -version
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| -consulta `[[[<drive>:]<path>]<query.xml>]` | Muestra una lista de todas las funciones, servicios de función y características que están instalados y disponibles para su instalación en el servidor. También puede usar la forma abreviada de este parámetro, **-q**. Si desea guardar los resultados de la consulta en un archivo XML, especifique un archivo XML para reemplazarlo `<query.xml>` . |
| -inputPath  `[[[<drive>:]<path>]<answer.xml>]` | Instala o quita los roles, servicios de función y características especificados en un archivo de respuesta XML representado por `<answer.xml>` . También puede usar la forma abreviada de este parámetro, **-p.** |
| -instalar `<id>` | Instala el rol, el servicio de función o la característica especificados por `<id>` . Los identificadores no distinguen mayúsculas de minúsculas. Varios roles, servicios de rol y características deben estar separados por espacios. Los siguientes parámetros opcionales se usan con el parámetro **-install** :<ul><li>**: configuración** `<SettingName>=<SettingValue>` -Especifica la configuración necesaria para la instalación.</li><li>**-allSubFeatures** : especifica la instalación de todos los servicios y características subordinados junto con el rol primario, el servicio de rol o la característica denominada en el `<id>` valor.<p>**NOTA**:<br>Algunos contenedores de roles no tienen un identificador de línea de comandos para permitir la instalación de todos los servicios de rol. Esto sucede, por ejemplo, cuando no pueden instalarse servicios de función en la misma instancia del comando del Administrador del servidor. Por ejemplo, la Servicio de federación servicio de rol de servicios de Federación de Active Directory y el servicio de rol Proxy de Servicio de federación no se pueden instalar con la misma instancia de comando Administrador del servidor.</li><li>**-resultpath** `<result.xml>` : Guarda los resultados de la instalación en un archivo XML representado por `<result.xml>` . También puede usar la forma abreviada de este parámetro, **-r**.<p>**NOTA**:<br>No se puede ejecutar ServerManagerCmd con el parámetro **-resultpath** y el parámetro **-Whatif** especificado.</li><li>**-restart** : reinicia el equipo automáticamente cuando se completa la instalación (si los roles o las características instaladas requieren un reinicio).</li><li>**-Whatif** : muestra todas las operaciones especificadas para el parámetro **-install** . También puede usar la forma abreviada del parámetro **-Whatif** , **-w**. No se puede ejecutar **ServerManagerCmd** con el parámetro **-resultpath** y el parámetro **-Whatif** especificado.</li><li>**-LogPath** `<[[<drive>:]<path>]<log.txt>>` : Especifica un nombre y una ubicación para el archivo de registro que no es el predeterminado `%windir%\temp\servermanager.log` .</li></ul> |
| -quitar `<id>` | Quita el rol, el servicio de función o la característica especificados por `<id>` . Los identificadores no distinguen mayúsculas de minúsculas. Varios roles, servicios de rol y características deben estar separados por espacios. Los siguientes parámetros opcionales se usan con el parámetro **-Remove** :<ul><li>**-resultpath** `<[[<drive>:]<path>]result.xml>` : Guarda los resultados de la eliminación en un archivo XML representado por `<result.xml>` . También puede usar la forma abreviada de este parámetro, **-r**.<p>**NOTA**:<br>No se puede ejecutar ServerManagerCmd con los parámetros **-resultpath** y **-Whatif** especificados.</li><li>**-restart** : reinicia el equipo automáticamente cuando se completa la eliminación (si el reinicio es necesario para los roles o las características restantes).</li><li>**-Whatif** : muestra todas las operaciones especificadas para el parámetro-Remove. También puede usar la forma abreviada del parámetro-Whatif, **-w**. No se puede ejecutar ServerManagerCmd con los parámetros **-resultpath** y **-Whatif** especificados.</li><li>**-LogPath** `<[[<Drive>:]<path>]<log.txt>>` : Especifica un nombre y una ubicación para el archivo de registro que no es el predeterminado `%windir%\temp\servermanager.log` .</li></ul> |
| -version | Muestra el número de versión del Administrador del servidor. También puede usar la forma abreviada, **-v**. |
| -help | Muestra la ayuda en la ventana del símbolo del sistema. También puede usar la forma abreviada, **-?**. |

## <a name="examples"></a>Ejemplos

Para mostrar una lista de todas las funciones, servicios de función y características disponibles y las funciones, servicios de función y características que están instalados en el equipo, escriba:

```
servermanagercmd -query
```

Para instalar el rol servidor Web (IIS) y guardar los resultados de la instalación en un archivo XML representado por *installResult.xml*, escriba:

```
servermanagercmd -install Web-Server -resultpath installResult.xml
```

Para mostrar información detallada acerca de los roles, servicios de función y características que se instalarán o quitarán, en función de las instrucciones especificadas en un archivo de respuesta XML representado por *install.xml*, escriba:

```
servermanagercmd -inputpath install.xml -whatif
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Información general de Administrador del servidor](/administration/server-manager/server-manager)
