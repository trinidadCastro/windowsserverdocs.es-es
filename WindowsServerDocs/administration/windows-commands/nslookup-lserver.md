---
title: nslookup lserver
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2f2f787915f2b941d6c098d44de1bb0e04dbd491
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436910"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el servidor predeterminado para el dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                      Descripción                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica el nuevo dominio DNS para el servidor predeterminado.  |
| {help &#124; ?} | Muestra un resumen breve de **nslookup** subcomandos. |

## <a name="remarks"></a>Comentarios
- El **lserver** comando utiliza el servidor inicial para buscar la información sobre el dominio DNS especificado. Esto es por el contrario el **server** comando, que usa el servidor predeterminado actual.
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup server](nslookup-server.md)
