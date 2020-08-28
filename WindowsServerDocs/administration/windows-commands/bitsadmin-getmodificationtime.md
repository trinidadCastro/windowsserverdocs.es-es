---
title: bitsadmin getmodificationtime
description: Artículo de referencia para el comando bitsadmin getmodificationtime, que recupera la última vez que se modificó el trabajo o cuando los datos se transfirieron correctamente.
ms.topic: reference
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 763ca0f60fa834ad14b92eb7963107fa407a8964
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028733"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

Recupera la última vez que se modificó el trabajo o los datos se transfirieron correctamente.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar la hora de la última modificación del trabajo denominado *myDownloadJob*:

```
bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
