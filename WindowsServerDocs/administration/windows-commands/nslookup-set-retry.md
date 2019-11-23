---
title: nslookup set retry
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 306bcc4f5e7ac98767c3c2e274100cf917874a8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372857"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el número de reintentos.
## <a name="syntax"></a>Sintaxis
```
set retry=<Number>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                      Descripción                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Especifica el nuevo valor para el número de reintentos. El número predeterminado de reintentos es 4. |
| {ayuda &#124; ?} |                 Muestra un breve resumen de los subcomandos de **nslookup** .                  |

## <a name="remarks"></a>Observaciones
- Cuando no se recibe una respuesta a una solicitud en un período de tiempo determinado, el tiempo de espera se duplica y se reenvía la solicitud. El valor de reintento controla el número de veces que se reenvía una solicitud antes de abandonarla. Puede cambiar el período de tiempo de espera con el subcomando **establecer tiempo de espera** .
  ## <a name="additional-references"></a>referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup Set timeout](nslookup-set-timeout.md)
