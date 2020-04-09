---
title: nslookup server
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8de2d8a801d57a9197d5e8ec7d3b6adb2d00dd04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838688"
---
# <a name="nslookup-server"></a>nslookup server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
server <DNSDomain>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                          Descripción                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obligatorio. Especifica el nuevo dominio DNS para el servidor predeterminado. |
| {ayuda &#124; ?} |     Muestra un breve resumen de los subcomandos de **nslookup** .      |

## <a name="remarks"></a>Comentarios
- El comando **Server** usa el servidor predeterminado actual para buscar la información sobre el dominio DNS especificado. Esto contrasta con el comando **lserver** , que utiliza el servidor inicial.
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
