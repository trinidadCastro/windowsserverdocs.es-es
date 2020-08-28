---
title: ftp delete
description: Artículo de referencia del comando FTP Delete, que elimina archivos en equipos remotos.
ms.topic: reference
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebf1a770144409dca91ddea0a18a85536e05b926
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038063"
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
