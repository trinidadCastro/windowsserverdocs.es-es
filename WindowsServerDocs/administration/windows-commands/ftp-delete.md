---
title: ftp delete
description: Artículo de referencia del comando FTP Delete, que elimina archivos en equipos remotos.
ms.topic: reference
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7a09e9abbe23582a2b5f2ba197a1877ffc62c7cc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624796"
---
# <a name="ftp-delete"></a>ftp delete

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina archivos en equipos remotos.

## <a name="syntax"></a>Sintaxis

```
delete <remotefile>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<remotefile>` | Especifica el archivo que se va a eliminar. |

### <a name="examples"></a>Ejemplos

Para eliminar el archivo de *test.txt* en el equipo remoto, escriba:

```
delete test.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
