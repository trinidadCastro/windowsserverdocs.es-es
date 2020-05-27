---
title: mput FTP
description: Tema de referencia para el comando FTP mput, que copia archivos locales en el equipo remoto mediante el tipo de transferencia de archivos actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5006c1ba19f0e017dea377b47bd0d89a68266382
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820415"
---
# <a name="ftp-mput"></a>mput FTP

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia los archivos locales en el equipo remoto mediante el tipo de transferencia de archivos actual.

## <a name="syntax"></a>Sintaxis

```
mput <localfile>[ ]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<localfile>` | Especifica el archivo local que se copiará en el equipo remoto. |

### <a name="examples"></a>Ejemplos

Para copiar *Program1. exe* y *Program2. exe* en el equipo remoto mediante el tipo de transferencia de archivo actual, escriba:

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando ASCII de FTP](ftp-ascii.md)

- [comando binario de FTP](ftp-binary.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
