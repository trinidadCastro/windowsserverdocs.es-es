---
title: bitsadmin gethttpmethod
description: Artículo de referencia para el comando bitsadmin gethttpmethod, que obtiene el verbo HTTP que se va a usar con el trabajo.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: ea6f1b3896b11bfcc157ea54dc0470786d3dff1f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894228"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtiene el verbo HTTP que se va a usar con el trabajo.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar el verbo HTTP que se va a usar con el trabajo denominado *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
