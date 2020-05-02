---
title: nslookup set root
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d913669fd4fede06c9983756df1bbf626ca430ac
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723576"
---
# <a name="nslookup-set-root"></a>nslookup set root

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el nombre del servidor raíz que se usa para las consultas.
## <a name="syntax"></a>Sintaxis
```
set root=<RootServer>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                   Descripción                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Especifica el nuevo nombre del servidor raíz. El valor predeterminado es ns.nic.ddn.mil. |
| {ayuda &#124;?} |              Muestra un breve resumen de los subcomandos de **nslookup** .               |

## <a name="remarks"></a>Observaciones
- El subcomando **establecer raíz** afecta al subcomando **raíz** .
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de la línea de comandos](command-line-syntax-key.md)
  [nslookup raíz nslookup](nslookup-root.md)
