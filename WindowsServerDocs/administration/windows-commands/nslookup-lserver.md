---
title: nslookup lserver
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d0d8619101d2e7b1f7fb6d6ed99d801c7c264f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838638"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                      Descripción                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica el nuevo dominio DNS para el servidor predeterminado.  |
| {ayuda &#124; ?} | Muestra un breve resumen de los subcomandos de **nslookup** . |

## <a name="remarks"></a>Comentarios
- El comando **lserver** usa el servidor inicial para buscar información sobre el dominio DNS especificado. Esto contrasta con el comando de **servidor** , que utiliza el servidor predeterminado actual.
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [servidor nslookup](nslookup-server.md)
