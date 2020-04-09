---
title: nslookup set timeout
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8506511fc203f94d395851471f6a981ef0765928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838278"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el número inicial de segundos de espera para una respuesta a una solicitud de búsqueda.
## <a name="syntax"></a>Sintaxis
```
set timeout=<Number>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                           Descripción                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Especifica el número de segundos que se va a esperar una respuesta. El número predeterminado de segundos de espera es 5. |
| {ayuda &#124; ?} |                      Muestra un breve resumen de los subcomandos de **nslookup** .                       |

## <a name="remarks"></a>Comentarios
- Cuando no se recibe una respuesta a una solicitud dentro del período de tiempo especificado, se duplica el tiempo de espera y se envía de nuevo la solicitud. Puede usar el comando **establecer reintento** para controlar el número de reintentos.
  ## <a name="examples"></a><a name=BKMK_examples></a>Example
  En el ejemplo siguiente se establece el tiempo de espera para obtener una respuesta a 2 segundos:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup set retry](nslookup-set-retry.md)
