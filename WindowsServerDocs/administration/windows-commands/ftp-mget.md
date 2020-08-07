---
title: ftp mget
description: Artículo de referencia para el comando FTP mget, que copia archivos remotos en el equipo local con el tipo de transferencia de archivos actual.
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9074df66e70961c74ef1b479f31ac316e34ff051
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889336"
---
# <a name="ftp-mget"></a>ftp mget

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia los archivos remotos en el equipo local mediante el tipo de transferencia de archivos actual.

## <a name="syntax"></a>Sintaxis

```
mget <remotefile>[ ]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica los archivos remotos que se van a copiar en el equipo local. |

### <a name="examples"></a>Ejemplos

Para copiar los archivos remotos *a.exe* y *b.exe* en el equipo local con el tipo de transferencia de archivo actual, escriba:

```
mget a.exe b.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando ASCII de FTP](ftp-ascii.md)

- [comando binario de FTP](ftp-binary.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
