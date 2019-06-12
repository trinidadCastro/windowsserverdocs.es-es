---
title: nslookup set root
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 38cd5a2e9878a8e43393befc5cbd4fc47c65ec53
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436604"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre del servidor raíz utilizado para consultas.
## <a name="syntax"></a>Sintaxis
```
set root=<RootServer>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                   Descripción                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Especifica el nuevo nombre para el servidor raíz. El valor predeterminado es ns.nic.ddn.mil. |
| {help &#124; ?} |              Muestra un resumen breve de **nslookup** subcomandos.               |

## <a name="remarks"></a>Comentarios
- El **conjunto raíz** subcomando afecta a la **raíz** subcomando.
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup raíz](nslookup-root.md)
