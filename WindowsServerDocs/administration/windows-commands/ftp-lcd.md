---
title: ftp lcd
description: Artículo de referencia del comando LCD de FTP, que cambia el directorio de trabajo en el equipo local.
ms.topic: reference
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 344d0ba14c552315504fc16b109500141c341f97
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038926"
---
# <a name="ftp-lcd"></a>ftp lcd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el directorio de trabajo en el equipo local. De forma predeterminada, el directorio de trabajo es el directorio en el que se inició el comando **FTP** .

## <a name="syntax"></a>Sintaxis

```
lcd [<directory>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[<directory>]` | Especifica el directorio del equipo local al que se va a cambiar. Si no se especifica *Directory* , el directorio de trabajo actual se cambia al directorio predeterminado. |

### <a name="examples"></a>Ejemplos

Para cambiar el directorio de trabajo en el equipo local a *c:\dir1*, escriba:

```
lcd c:\dir1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
