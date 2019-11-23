---
title: nslookup set timeout
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32fcfcaeccb6599e9aaca21f9c085bb00857479c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372756"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el número inicial de segundos de espera para una respuesta a una solicitud de búsqueda.
## <a name="syntax"></a>Sintaxis
```
set timeout=<Number>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                           Descripción                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Especifica el número de segundos que se va a esperar una respuesta. El número predeterminado de segundos de espera es 5. |
| {ayuda &#124; ?} |                      Muestra un breve resumen de los subcomandos de **nslookup** .                       |

## <a name="remarks"></a>Observaciones
- Cuando no se recibe una respuesta a una solicitud dentro del período de tiempo especificado, se duplica el tiempo de espera y se envía de nuevo la solicitud. Puede usar el comando **establecer reintento** para controlar el número de reintentos.
  ## <a name="BKMK_examples"></a>Example
  En el ejemplo siguiente se establece el tiempo de espera para obtener una respuesta a 2 segundos:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup set retry](nslookup-set-retry.md)
