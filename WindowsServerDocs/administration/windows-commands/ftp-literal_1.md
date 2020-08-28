---
title: ftp literal
description: Artículo de referencia para el comando FTP literal, que envía argumentos textuales al servidor FTP remoto.
ms.topic: reference
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff0f322d3b2ff63705077165ad73f58830cee216
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023869"
---
# <a name="ftp-literal"></a>ftp literal

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía argumentos textuales al servidor FTP remoto. Se devuelve un solo código de respuesta FTP.

> [!NOTE]
> Este comando es el mismo que el [comando de Comillas FTP](ftp-quote.md).

## <a name="syntax"></a>Sintaxis

```
literal <argument> [ ]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<argument>` | Especifica el argumento que se va a enviar al servidor FTP. |

### <a name="examples"></a>Ejemplos

Para enviar un comando **Quit** al servidor FTP remoto, escriba:

```
literal quit
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de Comillas FTP](ftp-quote.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
