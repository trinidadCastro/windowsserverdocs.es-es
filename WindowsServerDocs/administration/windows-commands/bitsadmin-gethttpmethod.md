---
title: bitsadmin gethttpmethod
description: Artículo de referencia para el comando bitsadmin gethttpmethod, que obtiene el verbo HTTP que se va a usar con el trabajo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3a963eb275c6e635849094906c52fedf90dccd0d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927021"
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
