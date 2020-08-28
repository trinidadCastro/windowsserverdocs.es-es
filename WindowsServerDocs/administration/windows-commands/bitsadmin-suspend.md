---
title: bitsadmin suspend
description: Artículo de referencia del comando bitsadmin Suspend, que suspende el trabajo especificado.
ms.topic: reference
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad4ca877ac9e5548f3d3ec830f8fc2822f781218
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033393"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Suspende el trabajo especificado. Si suspendió el trabajo por error, puede usar el modificador de la [reanudación de bitsadmin](bitsadmin-resume.md) para reiniciar el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ---------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="example"></a>Ejemplo

Para suspender el trabajo denominado *myDownloadJob*:


```
bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de reanudación de bitsadmin](bitsadmin-resume.md)

- [bitsadmin (comando)](bitsadmin.md)
