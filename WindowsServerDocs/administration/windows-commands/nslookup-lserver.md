---
title: nslookup lserver
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 347ad6e380f8d8163c4954771c9e985271b2d549
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373081"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                      Descripción                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica el nuevo dominio DNS para el servidor predeterminado.  |
| {ayuda &#124; ?} | Muestra un breve resumen de los subcomandos de **nslookup** . |

## <a name="remarks"></a>Comentarios
- El comando **lserver** usa el servidor inicial para buscar información sobre el dominio DNS especificado. Esto contrasta con el comando de **servidor** , que utiliza el servidor predeterminado actual.
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)[nslookup Server](nslookup-server.md) 
  
