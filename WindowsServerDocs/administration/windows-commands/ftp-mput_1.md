---
title: ftp mput
description: Artículo de referencia para el comando FTP mput, que copia archivos locales en el equipo remoto mediante el tipo de transferencia de archivos actual.
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2caf7e91341f470fc265d0d1dbf6a51b6fe99bb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889229"
---
# <a name="ftp-mput"></a>ftp mput

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

Para copiar *Program1.exe* y *Program2.exe* en el equipo remoto con el tipo de transferencia de archivo actual, escriba:

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando ASCII de FTP](ftp-ascii.md)

- [comando binario de FTP](ftp-binary.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
