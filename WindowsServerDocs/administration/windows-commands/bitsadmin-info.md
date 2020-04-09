---
title: bitsadmin info
description: Tema de comandos de Windows para la **información de bitsadmin**, que muestra información de resumen sobre el trabajo especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9d8b840e39620db01c44001d9cb7d593be2be5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850348"
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

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera información sobre el trabajo denominado *myDownloadJob*.

```
C:\>bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)