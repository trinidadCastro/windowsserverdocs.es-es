---
title: nslookup set search
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9972919eae1be21d5dd30820d64dd1576b935666
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838308"
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
|  **buscan**  | Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. La sintaxis predeterminada es **Search**. |
|    {ayuda     |                                                                              ?}                                                                               |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)