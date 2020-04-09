---
title: memoria caché de bitsadmin y setexpirationtime
description: Tema de comandos de Windows para la **caché de bitsadmin y setexpirationtime**, que establece la hora de expiración de la caché.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf283a0a8b94fd55c591609e3dcd1d127a2be81a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850888"
---
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>memoria caché de bitsadmin y setexpirationtime

Establece la hora de expiración de la caché.

## <a name="syntax"></a>Sintaxis

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| -------------- | -------------- |
| segundos | El número de segundos hasta que expire la memoria caché. |

## <a name="examples"></a><a name=BKMK_examples></a>Example

En el ejemplo siguiente se expira la caché en 60 segundos.

```
C:\>bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
