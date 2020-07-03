---
title: bitsadmin makecustomheaderswriteonly
description: Artículo de referencia para el comando bitsadmin makecustomheaderswriteonly, que hace que los encabezados HTTP personalizados de un trabajo sean de solo escritura.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3a97054bb9e8156de23e07d18d9806e04c59e967
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926522"
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
