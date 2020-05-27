---
title: mdelete FTP
description: Tema de referencia del comando FTP mdelete, que elimina archivos del equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8ee1882878ce06a16bd6ff6f0dcaa512d6d8b56a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820235"
---
# <a name="ftp-mdelete"></a>mdelete FTP

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

Para eliminar los archivos remotos *a. exe* y *b. exe*, escriba:

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
