---
title: literal de FTP
description: Tema de referencia del comando FTP literal, que envía argumentos textuales al servidor FTP remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5015f2184c9273aae6dbd01b18ee1f540d5b9aa3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820165"
---
# <a name="ftp-literal"></a>literal de FTP

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

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
