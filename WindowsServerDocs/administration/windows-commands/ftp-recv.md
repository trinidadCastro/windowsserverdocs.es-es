---
title: ftp recv
description: Artículo de referencia para el comando recepción de FTP, que copia un archivo remoto en el equipo local con el tipo de transferencia de archivos actual.
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 888acc77ec4f6edc57d9d1bed76563538f621eb5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889050"
---
# <a name="ftp-recv"></a>ftp recv

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Copia un archivo remoto en el equipo local mediante el tipo de transferencia de archivos actual.

> [!NOTE]
> Este comando es el mismo que el [comando get de FTP](ftp-get.md).

## <a name="syntax"></a>Sintaxis

```
recv <remotefile> [<localfile>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica el archivo remoto que se va a copiar. |
| `[<localfile>]` | Especifica el nombre del archivo que se va a usar en el equipo local. Si no se especifica *archivolocal* , el archivo recibe el nombre del *archivoremoto*. |

### <a name="examples"></a>Ejemplos

Para copiar *test.txt* en el equipo local mediante la transferencia de archivos actual, escriba:

```
recv test.txt
```

Para copiar *test.txt* en el equipo local como *test1.txt* mediante la transferencia de archivos actual, escriba:

```
recv test.txt test1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando get de FTP](ftp-get.md)

- [comando ASCII de FTP](ftp-ascii.md)

- [comando binario de FTP](ftp-binary.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
