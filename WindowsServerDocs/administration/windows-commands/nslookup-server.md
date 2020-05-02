---
title: nslookup server
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9a3f4c7bdbcf8122bb532fafe83b400400d8d6b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723682"
---
# <a name="nslookup-server"></a>nslookup server

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
server <DNSDomain>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                          Descripción                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Necesario. Especifica el nuevo dominio DNS para el servidor predeterminado. |
| {ayuda &#124;?} |     Muestra un breve resumen de los subcomandos de **nslookup** .      |

## <a name="remarks"></a>Observaciones
- El comando **Server** usa el servidor predeterminado actual para buscar la información sobre el dominio DNS especificado. Esto contrasta con el comando **lserver** , que utiliza el servidor inicial.
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
