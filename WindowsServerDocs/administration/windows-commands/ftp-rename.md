---
title: ftp rename
description: Artículo de referencia del comando Rename de FTP, que cambia el nombre de los archivos remotos.
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85903987be9df566f4c07bc7fb5b96e76b0aa43
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888982"
---
# <a name="ftp-rename"></a>ftp rename

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre de los archivos remotos.

## <a name="syntax"></a>Sintaxis

```
rename <filename> <newfilename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<filename>` | Especifica el archivo cuyo nombre desea cambiar. |
| `<newfilename>` | Especifica el nuevo nombre de archivo. |

### <a name="examples"></a>Ejemplos

Para cambiar el nombre del archivo remoto *example.txt* a *example1.txt*, escriba:

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Guía de FTP adicional](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
