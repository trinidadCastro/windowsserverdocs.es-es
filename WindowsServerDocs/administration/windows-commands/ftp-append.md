---
title: ftp append
description: Artículo de referencia del comando APPEND de FTP, que anexa un archivo local a un archivo en el equipo remoto con la configuración de tipo de archivo actual.
ms.topic: reference
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 42270b8f3633158e12d472a234fcf1904cee86de
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638576"
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
