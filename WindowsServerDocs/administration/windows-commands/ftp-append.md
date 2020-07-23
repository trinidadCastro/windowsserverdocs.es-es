---
title: ftp append
description: Artículo de referencia del comando APPEND de FTP, que anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cee11d52db784477b0b4ffeecc307d395f910a92
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958087"
---
# <a name="ftp-append"></a>ftp append

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.

## <a name="syntax"></a>Sintaxis

```
append <localfile> [remotefile]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<localfile>` | Especifica el archivo local que se va a agregar. |
| archivoremoto | Especifica el archivo en el equipo remoto al que <localfile> se agrega. Si no usa este parámetro, `<localfile>` se usa el nombre en lugar del nombre de archivo remoto. |

### <a name="examples"></a>Ejemplos

Para anexar *file1.txt* a *file2.txt* en el equipo remoto, escriba:

```
append file1.txt file2.txt
```

Para anexar el *file1.txt* local a un archivo denominado *file1.txt* en el equipo remoto.

```
append file1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
