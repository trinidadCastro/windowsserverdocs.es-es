---
title: bitsadmin util y enableanalyticchannel
description: Artículo de referencia del comando bitsadmin util y enableanalyticchannel, que habilita o deshabilita el canal de análisis de cliente de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 515402f42dee54baa662f37718841f70b1cd2882
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927414"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util y enableanalyticchannel

Habilita o deshabilita el canal de análisis de cliente de BITS.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Parámetro | Descripción |
| --------- | ---------- |
| TRUE o FALSE | **True** activa la validación de contenido para el archivo especificado, mientras que **false** lo desactiva. |

## <a name="examples"></a>Ejemplos

Para activar o desactivar el canal de análisis de cliente de BITS.

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin util (comando)](bitsadmin-util.md)

- [bitsadmin (comando)](bitsadmin.md)
