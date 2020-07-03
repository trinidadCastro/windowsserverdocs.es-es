---
title: ayuda
description: Artículo de referencia del comando ayuda, que muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65b5ac3-711a-4368-95b8-ba82e2d00713
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 562bb725998cb58eb9a4a9ce9078a833bc0e7781
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924578"
---
# <a name="help"></a>ayuda

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado. Si se usa sin parámetros, las listas de **ayuda** y describen brevemente cada comando del sistema.

## <a name="syntax"></a>Sintaxis

```
help [<command>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<command>` | Especifica el comando para el que se va a mostrar información de ayuda detallada. |

### <a name="examples"></a>Ejemplos

Para ver información acerca del comando **Robocopy** , escriba:

```
help robocopy
```

Para mostrar una lista de todos los comandos disponibles en DiskPart, escriba:

```
help
```

Para mostrar información detallada sobre la ayuda sobre cómo usar el comando **Create Partition Primary** en DiskPart, escriba:

```
help create partition primary
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
