---
title: nslookup set root
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea2c34bbf7c9323c948d57ac2a838c22aea1008e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838318"
---
# <a name="nslookup-set-root"></a>nslookup set root

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el nombre del servidor raíz que se usa para las consultas.
## <a name="syntax"></a>Sintaxis
```
set root=<RootServer>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                   Descripción                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Especifica el nuevo nombre del servidor raíz. El valor predeterminado es ns.nic.ddn.mil. |
| {ayuda &#124; ?} |              Muestra un breve resumen de los subcomandos de **nslookup** .               |

## <a name="remarks"></a>Comentarios
- El subcomando **establecer raíz** afecta al subcomando **raíz** .
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [raíz de nslookup](nslookup-root.md)
