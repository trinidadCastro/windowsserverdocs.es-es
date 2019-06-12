---
title: nslookup set search
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d95ebe30ce45430787bebbfe63766a571a436bbf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436593"
---
# <a name="nslookup-set-search"></a>nslookup set search



Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominio DNS a la solicitud hasta que se reciba una respuesta. Esto se aplica cuando el conjunto y la búsqueda de solicitan contener al menos un punto, pero no finalizan con un punto final.

## <a name="syntax"></a>Sintaxis

```
set [no]search
```

## <a name="parameters"></a>Parámetros

|  Parámetro   |                                                                          Descripción                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Deja de anexar los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominio DNS a la solicitud.                            |
|  **search**  | Anexa los nombres de dominio del sistema de nombres de dominio (DNS) en la lista de búsqueda de dominio DNS a la solicitud hasta que se reciba una respuesta. La sintaxis predeterminada es **búsqueda**. |
|    {Ayuda     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)