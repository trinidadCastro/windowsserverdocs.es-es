---
title: bitsadmin makecustomheaderswriteonly
description: Artículo de referencia para el comando bitsadmin makecustomheaderswriteonly, que hace que los encabezados HTTP personalizados de un trabajo sean de solo escritura.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 4d31f51c2531079342e223752c626b0b7e8d19f8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024269"
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
