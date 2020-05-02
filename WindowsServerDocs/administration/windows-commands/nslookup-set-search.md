---
title: nslookup set search
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e3f5bce42d3614b535b2dfb00c4c9ea9cac2346
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723565"
---
# <a name="nslookup-set-search"></a>nslookup set search



Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. Esto se aplica cuando el conjunto y la solicitud de búsqueda contienen al menos un punto, pero no finalizan con un punto final.

## <a name="syntax"></a>Sintaxis

```
set [no]search
```

### <a name="parameters"></a>Parámetros

|  Parámetro   |                                                                          Descripción                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Deja de anexar los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud.                            |
|  **search**  | Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. La sintaxis predeterminada es **Search**. |
|    {ayuda     |                                                                              ?}                                                                               |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)