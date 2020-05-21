---
title: fsutil resource
description: Tema de referencia para el comando fsutil Resource, que administra un Administrador de recursos transaccional y su comportamiento.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b64dab3a0d0e772f067b0d3fa548692aad38a4c5
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435740"
---
# <a name="fsutil-resource"></a>fsutil resource

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Crea un Administrador de recursos transaccional secundario, inicia o detiene una Administrador de recursos transaccional, o muestra información sobre un Administrador de recursos transaccional, y modifica el comportamiento siguiente:

- Si un Administrador de recursos transaccional predeterminado limpia sus metadatos transaccionales en el siguiente montaje.

- Administrador de recursos transaccional especificado para preferir la coherencia sobre la disponibilidad.

- La transacción especificada Administrador de recursos para preferir la disponibilidad a través de la coherencia.

- Características de una Administrador de recursos transaccional en ejecución.

## <a name="syntax"></a>Sintaxis

```
fsutil resource [create] <rmrootpathname>
fsutil resource [info] <rmrootpathname>
fsutil resource [setautoreset] {true|false} <Defaultrmrootpathname>
fsutil resource [setavailable] <rmrootpathname>
fsutil resource [setconsistent] <rmrootpathname>
fsutil resource [setlog] [growth {<containers> containers|<percent> percent} <rmrootpathname>] [maxextents <containers> <rmrootpathname>] [minextents <containers> <rmrootpathname>] [mode {full|undo} <rmrootpathname>] [rename <rmrootpathname>] [shrink <percent> <rmrootpathname>] [size <containers> <rmrootpathname>]
fsutil resource [start] <rmrootpathname> [<rmlogpathname> <tmlogpathname>
fsutil resource [stop] <rmrootpathname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| create | Crea un Administrador de recursos transaccional secundario. |
| `<rmrootpathname>` | Especifica la ruta de acceso completa a un directorio raíz de Administrador de recursos transaccional. |
| info | Muestra la información del Administrador de recursos transaccional especificado. |
| setautoreset | Especifica si un Administrador de recursos transaccional predeterminado limpiará los metadatos transaccionales en el siguiente montaje.<ul><li>**true** : especifica que el administrador de recursos de transacciones Limpie los metadatos transaccionales en el siguiente montaje, de forma predeterminada.</li><li>**false** : especifica que la transacción administrador de recursos no limpiará los metadatos transaccionales en el siguiente montaje, de forma predeterminada. |
| `<defaultrmrootpathname>` | Especifica el nombre de la unidad seguido de dos puntos. |
| setavailable | Especifica que un Administrador de recursos transaccional preferirá la disponibilidad a través de la coherencia. |
| setconsistent | Especifica que un Administrador de recursos transaccional preferirá la coherencia con respecto a la disponibilidad. |
| Setlog | Cambia las características de un Administrador de recursos transaccional que ya se está ejecutando. |
| growth | Especifica la cantidad por la que puede aumentar el registro de Administrador de recursos transaccional.<p>El parámetro Growth se puede especificar de la manera siguiente:<ul><li>Número de contenedores, con el formato:`<containers> containers`</li><li>Porcentaje, con el formato:`<percent> percent`</li></ul> |
| `<containers>` | Especifica los objetos de datos utilizados por el Administrador de recursos transaccional. |
| maxextent | Especifica el número máximo de contenedores para el Administrador de recursos transaccional especificado. |
| minextent | Especifica el número mínimo de contenedores para el Administrador de recursos transaccional especificado. |
| modo`{full|undo}` | Especifica si todas las transacciones están registradas ( **completo**) o solo se registran los eventos revertidos (**Deshacer**). |
| rename | Cambia el GUID del Administrador de recursos transaccional. |
| shrink | Especifica el porcentaje por el que se puede reducir automáticamente el registro de Administrador de recursos transaccionales. |
| tamaño | Especifica el tamaño de la Administrador de recursos transaccional como un número especificado de *contenedores*. |
| start | Inicia el Administrador de recursos transaccional especificado. |
| stop | Detiene el Administrador de recursos transaccional especificado. |

### <a name="examples"></a>Ejemplos

Para establecer el registro del Administrador de recursos transaccional que se especifica en *c:\test*, para tener un crecimiento automático de cinco contenedores, escriba:

```
fsutil resource setlog growth 5 containers c:test
```

Para establecer el registro del Administrador de recursos transaccional que se especifica en *c:\test*, para que tenga un crecimiento automático de dos por ciento, escriba:

```
fsutil resource setlog growth 2 percent c:test
```

Para especificar que el Administrador de recursos transaccional predeterminado Limpie los metadatos transaccionales en el siguiente montaje de la unidad C, escriba:

```
fsutil resource setautoreset true c:\
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS de transacciones](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730726(v=ws.10))
