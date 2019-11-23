---
title: nslookup server
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ce609f62f2d87024e1d75b43869b3b867e946e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373059"
---
# <a name="nslookup-server"></a>nslookup server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
server <DNSDomain>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                          Descripción                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obligatorio. Especifica el nuevo dominio DNS para el servidor predeterminado. |
| {ayuda &#124; ?} |     Muestra un breve resumen de los subcomandos de **nslookup** .      |

## <a name="remarks"></a>Observaciones
- El comando **Server** usa el servidor predeterminado actual para buscar la información sobre el dominio DNS especificado. Esto contrasta con el comando **lserver** , que utiliza el servidor inicial.
  ## <a name="additional-references"></a>referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
