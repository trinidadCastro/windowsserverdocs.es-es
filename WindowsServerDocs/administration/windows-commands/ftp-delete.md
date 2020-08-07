---
title: ftp delete
description: Artículo de referencia del comando FTP Delete, que elimina archivos en equipos remotos.
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 315a73f0ebfbefdf4a7033f42c2cad02e2ab77bc
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889528"
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
