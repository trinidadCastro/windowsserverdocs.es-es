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
ms.openlocfilehash: 5a1737275bf6321525bbba56cd4d6a77ef973423
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591029"
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

## <a name="remarks"></a>Observaciones
- El subcomando **establecer raíz** afecta al subcomando **raíz** .
  ## <a name="additional-references"></a>referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [raíz de nslookup](nslookup-root.md)
