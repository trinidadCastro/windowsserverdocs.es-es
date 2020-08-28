---
title: nslookup set search
description: Artículo de referencia del comando Nslookup Set Search, que anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud hasta que se recibe una respuesta.
ms.topic: reference
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17cee7cbc2112db346c91ab4a7b88264eff6a850
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025159"
---
# <a name="nslookup-set-search"></a>nslookup set search

Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. Esto se aplica cuando el conjunto y la solicitud de búsqueda contienen al menos un punto, pero no finalizan con un punto final.

## <a name="syntax"></a>Sintaxis

```
set [no]search
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| nosearch | Deja de anexar los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS para la solicitud. |
| búsqueda | Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS para la solicitud hasta que se reciba una respuesta. Este es el valor predeterminado. |
| /? | Muestra la ayuda en el símbolo del sistema. |
| /help | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
