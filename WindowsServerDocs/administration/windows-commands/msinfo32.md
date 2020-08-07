---
title: msinfo32
description: Artículo de referencia para el comando msinfo32, que abre la herramienta de información del sistema para mostrar una vista completa del hardware, los componentes del sistema y el entorno de software en el equipo local.
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29ad95b9772e7fbb738fb6d1f1eb3ff11de3b0c8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886163"
---
# <a name="msinfo32"></a>msinfo32

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Abre la herramienta información del sistema para mostrar una vista completa del hardware, los componentes del sistema y el entorno de software en el equipo local.

Algunas categorías de información del sistema contienen grandes cantidades de datos. Puede usar el comando **Start/wait** para optimizar el rendimiento de los informes de estas categorías. Para obtener más información, vea [información del sistema](/previous-versions/windows/it-pro/windows-server-2003/cc783305(v=ws.10)).

## <a name="syntax"></a>Sintaxis

```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computername>] [/showcategories] [/category <categoryID>] [/categories {+<categoryID>(+<categoryID>)|+all(-<categoryID>)}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<path>` | Especifica el archivo que se va a abrir en el formato *C:\Folder1\File1.xxx*, donde *C* es la letra de unidad, *carpeta1* es la carpeta, *archivo1* es el nombre de archivo y *XXX* es la extensión de nombre de archivo.<p>Este archivo puede ser un archivo **. nfo**, **. XML**, **. txt**o **. cab** . |
| `<computername>` | Especifica el nombre del equipo local o de destino. Puede ser un nombre UNC, una dirección IP o un nombre de equipo completo. |
| `<categoryID>` | Especifica el identificador del elemento de categoría. Puede obtener el identificador de categoría mediante **/showcategories**. |
| /pch | Muestra la vista historial del sistema en la herramienta información del sistema. |
| /NFO | Guarda el archivo exportado como un archivo **. nfo** . Si el nombre de archivo que se especifica en *path* no finaliza en una extensión **. nfo** , la extensión **. nfo** se anexa automáticamente al nombre de archivo. |
| /Report | Guarda el archivo en la *ruta de acceso* como un archivo de texto. El nombre de archivo se guarda exactamente como aparece en *ruta de acceso*. La extensión. txt no se anexa al archivo a menos que se especifique en ruta de acceso. |
| /Computer | Inicia la herramienta de información del sistema para el equipo remoto especificado. Debe tener los permisos adecuados para obtener acceso al equipo remoto. |
| /showcategories | Inicia la herramienta de información del sistema con todos los identificadores de categoría disponibles mostrados, en lugar de mostrar los nombres descriptivos o localizados. Por ejemplo, la categoría entorno de software se muestra como la categoría **SWEnv** . |
| /categoría | Inicia la información del sistema con la categoría especificada seleccionada. Use **/showcategories** para mostrar una lista de los identificadores de categoría disponibles. |
| /Categories | Inicia la información del sistema solo con la categoría o categorías especificadas que se muestran. También limita la salida a la categoría o categorías seleccionadas. Use **/showcategories** para mostrar una lista de los identificadores de categoría disponibles. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para enumerar los identificadores de categoría disponibles, escriba:

```
msinfo32 /showcategories
```

Para iniciar la herramienta información del sistema con toda la información disponible que se muestra, excepto los módulos cargados, escriba:

```
msinfo32 /categories +all -loadedmodules
```

Para mostrar información de **Resumen del sistema** y crear un archivo. nfo llamado *SYSSUM. nfo*, que contiene información en la categoría de **Resumen del sistema** , escriba:

```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```

Para mostrar la información de conflictos de recursos y crear un archivo. nfo llamado *Conflicts. nfo*, que contiene información sobre los conflictos de recursos, escriba:

```
msinfo32 /nfo conflicts.nfo /categories +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
