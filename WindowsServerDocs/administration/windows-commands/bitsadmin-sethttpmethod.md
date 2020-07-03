---
title: bitsadmin sethttpmethod
description: Artículo de referencia para el comando bitsadmin sethttpmethod, que establece el verbo HTTP que se va a usar.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 86d4749de294871a05176239cc1265974d8a7590
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927759"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

Establece el verbo HTTP que se va a usar.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| httpmethod | Verbo HTTP que se va a usar. Para obtener información sobre los verbos disponibles, vea [definiciones de método](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin (comando)](bitsadmin.md)
