---
title: bitsadmin getcompletiontime
description: Comando comandos de Windows para **bitsadmin getcompletiontime**, que recupera la hora en que el trabajo terminó de transferir los datos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5408e7e8c35135601a4a0af0ab7e9c55cea4c8dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850758"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Recupera la hora en que el trabajo finalizó la transferencia de datos.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| trabajo | El nombre para mostrar o el GUID del trabajo. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se recupera la hora en que el trabajo denominado *myDownloadJob* ha terminado de transferir los datos.

```
C:\>bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)