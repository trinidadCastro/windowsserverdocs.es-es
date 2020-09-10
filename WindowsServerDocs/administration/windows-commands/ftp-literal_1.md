---
title: ftp literal
description: Artículo de referencia para el comando FTP literal, que envía argumentos textuales al servidor FTP remoto.
ms.topic: reference
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67c881788e96ef825ad097e2ebd68ae20d753ebe
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621605"
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
