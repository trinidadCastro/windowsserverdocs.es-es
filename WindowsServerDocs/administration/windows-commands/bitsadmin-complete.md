---
title: bitsadmin complete
description: Artículo de referencia del comando bitsadmin complete, que completa el trabajo.
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43c3296520b5843643c10d204d89f4f2f2bf98d8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894605"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa el trabajo. Use este modificador después de que el trabajo se mueva al estado transferido. De lo contrario, solo estarán disponibles los archivos que se hayan transferido correctamente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="example"></a>Ejemplo

Para completar el trabajo *myDownloadJob* , una vez alcanzado el `TRANSFERRED` Estado:

```
bitsadmin /complete myDownloadJob
```

Si varios trabajos usan *myDownloadJob* como nombre, debe usar el GUID del trabajo para identificarlo de forma exclusiva para su finalización.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
