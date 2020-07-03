---
title: bitsadmin info
description: Artículo de referencia del comando bitsadmin info, que muestra información de resumen sobre el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b9a284ee1e0ab8501f0fb6bc3417ca399996a08
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926563"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Muestra información de resumen sobre el trabajo especificado.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |
| /verbose | Opcional. Proporciona información detallada acerca de cada trabajo. |

## <a name="examples"></a>Ejemplos

Para recuperar información sobre el trabajo denominado *myDownloadJob*:

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)
