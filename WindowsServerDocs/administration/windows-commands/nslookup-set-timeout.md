---
title: nslookup set timeout
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68e9630b9690c9b6c9d4c316f8b328897289362c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723542"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

cambia el número inicial de segundos de espera para una respuesta a una solicitud de búsqueda.
## <a name="syntax"></a>Sintaxis
```
set timeout=<Number>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                           Descripción                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Especifica el número de segundos que se va a esperar una respuesta. El número predeterminado de segundos de espera es 5. |
| {ayuda &#124;?} |                      Muestra un breve resumen de los subcomandos de **nslookup** .                       |

## <a name="remarks"></a>Observaciones
- Cuando no se recibe una respuesta a una solicitud dentro del período de tiempo especificado, se duplica el tiempo de espera y se envía de nuevo la solicitud. Puede usar el comando **establecer reintento** para controlar el número de reintentos.
  ## <a name="examples"></a>Ejemplos
  Para establecer el tiempo de espera para obtener una respuesta a 2 segundos:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup set retry](nslookup-set-retry.md)
