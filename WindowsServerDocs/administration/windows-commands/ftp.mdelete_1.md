---
title: ftp mdelete
description: Artículo de referencia para el comando FTP mdelete, que elimina archivos del equipo remoto.
ms.topic: reference
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 647f7aa532f15e5391f68ef2121766e9aa31ff85
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038896"
---
# <a name="ftp-mdelete"></a>ftp mdelete

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina archivos del equipo remoto.

## <a name="syntax"></a>Sintaxis
```
mdelete <remotefile>[...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica el archivo remoto que se va a eliminar. |

### <a name="examples"></a>Ejemplos

Para eliminar los archivos remotos *a.exe* y *b.exe*, escriba:

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
