---
title: nslookup set timeout
description: Artículo de referencia del comando Nslookup Set timeout, que cambia el número inicial de segundos de espera para una respuesta a una solicitud de búsqueda.
ms.topic: reference
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b470598378f1b338da209ea16a0d8102177c573c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640523"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el número inicial de segundos de espera para una respuesta a una solicitud de búsqueda. Si no se recibe una respuesta dentro del período de tiempo especificado, el tiempo de espera se duplica y se reenvía la solicitud. Use el comando [nslookup set retry](nslookup-set-retry.md) para determinar el número de veces que se intentará enviar la solicitud.

## <a name="syntax"></a>Sintaxis

```
set timeout=<number>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| ---------- | ---------- |
| `<number>` | Especifica el número de segundos que se va a esperar una respuesta. El número predeterminado de segundos de espera es **5**. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para establecer el tiempo de espera para obtener una respuesta a 2 segundos:

```
set timeout=2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [nslookup set retry](nslookup-set-retry.md)
