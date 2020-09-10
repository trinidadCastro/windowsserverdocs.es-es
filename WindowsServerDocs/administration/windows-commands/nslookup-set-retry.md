---
title: nslookup set retry
description: Artículo de referencia del comando Nslookup set retry, que establece el número de intentos para obtener información de un servidor especificado.
ms.topic: reference
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fa8b588c49b61c2269325b1b9e67d350f837c5ad
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637521"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Si no se recibe una respuesta dentro de un período de tiempo determinado, el tiempo de espera se duplica y se reenvía la solicitud. Este comando establece el número de veces que se reenvía una solicitud a un servidor para obtener información, antes de abandonarla.

> [!NOTE]
> Para cambiar el período de tiempo antes de que se agote el tiempo de espera de la solicitud, use el comando [nslookup Set timeout](nslookup-set-timeout.md) .

## <a name="syntax"></a>Sintaxis

```
set retry=<number>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---------- | ---------- |
| `<number>` | Especifica el nuevo valor para el número de reintentos. El número predeterminado de reintentos es **4**. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup set timeout](nslookup-set-timeout.md)
