---
title: nslookup set domain
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa433383e23fd19779960348e0af88e0a405ff83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838468"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre de dominio del sistema de nombres de dominio (DNS) predeterminado por el nombre especificado.
## <a name="syntax"></a>Sintaxis
```
set domain=<DomainName>
```
### <a name="parameters"></a>Parámetros

|    Parámetro    |                                           Descripción                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica un nuevo nombre para el nombre de dominio DNS predeterminado. El nombre de dominio predeterminado es el nombre de host. |
| {ayuda &#124; ?} |                      Muestra un breve resumen de los subcomandos de **nslookup** .                      |

## <a name="remarks"></a>Comentarios
- El nombre de dominio DNS predeterminado se anexa a una solicitud de búsqueda según el estado de las opciones **defname** y **Search** . La lista de búsqueda de dominios DNS contiene los elementos primarios del dominio DNS predeterminado si tiene al menos dos componentes en su nombre. Por ejemplo, si el dominio DNS predeterminado es mfg.widgets.com, la lista de búsqueda se denomina mfg.widgets.com y widgets.com. Use el comando **set srchlist** para especificar una lista diferente y el comando **set All** para mostrar la lista.
  ## <a name="additional-references"></a>Referencias adicionales
  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup Set srchlist](nslookup-set-srchlist.md)
  [nslookup SET ALL](nslookup-set-all.md)
