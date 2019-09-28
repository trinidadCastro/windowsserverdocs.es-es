---
title: nslookup set root
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08cf41ec9b6ac30699013112216a538dcf625fd5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372837"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el nombre del servidor raíz que se usa para las consultas.
## <a name="syntax"></a>Sintaxis
```
set root=<RootServer>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                   Descripción                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Especifica el nuevo nombre del servidor raíz. El valor predeterminado es ns.nic.ddn.mil. |
| {ayuda &#124; ?} |              Muestra un breve resumen de los subcomandos de **nslookup** .               |

## <a name="remarks"></a>Comentarios
- El subcomando **establecer raíz** afecta al subcomando **raíz** .
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [raíz de nslookup](nslookup-root.md)
