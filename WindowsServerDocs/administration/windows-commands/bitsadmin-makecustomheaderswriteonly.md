---
title: bitsadmin makecustomheaderswriteonly
description: Artículo de referencia para el comando bitsadmin makecustomheaderswriteonly, que hace que los encabezados HTTP personalizados de un trabajo sean de solo escritura.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b1e152ee51f3009a5cc1f5bf1b747e65e86e5e04
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893693"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Hacer que los encabezados HTTP personalizados de un trabajo sean de solo escritura.

> [!IMPORTANT]
> Esta acción no se puede deshacer.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para hacer que los encabezados HTTP personalizados sean de solo escritura para el trabajo denominado *myDownloadJob*:

```
bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
