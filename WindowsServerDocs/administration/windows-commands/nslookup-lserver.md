---
title: nslookup lserver
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2054c0fd427b41e7d6076258b29ab78d0fb7892
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723674"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                      Descripción                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Especifica el nuevo dominio DNS para el servidor predeterminado.  |
| {ayuda &#124;?} | Muestra un breve resumen de los subcomandos de **nslookup** . |

## <a name="remarks"></a>Observaciones
- El comando **lserver** usa el servidor inicial para buscar información sobre el dominio DNS especificado. Esto contrasta con el comando de **servidor** , que utiliza el servidor predeterminado actual.
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup Server](nslookup-server.md)
