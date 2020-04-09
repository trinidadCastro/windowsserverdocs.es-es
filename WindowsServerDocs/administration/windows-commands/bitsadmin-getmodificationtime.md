---
title: bitsadmin getmodificationtime
description: Comando comandos de Windows para **bitsadmin getmodificationtime**, que recupera la última vez que se modificó el trabajo o los datos se transfirieron correctamente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ace0f64b1fbe7ba72174bb3df2bd4dd65e929769
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850618"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la hora de la última modificación del trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)