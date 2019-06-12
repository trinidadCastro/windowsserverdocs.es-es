---
title: nslookup set domain
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1af9f30dd2c44111adecb477a6469333f4f7685
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436768"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cambia el nombre de dominio predeterminado del sistema de nombres de dominio (DNS) para el nombre especificado.
## <a name="syntax"></a>Sintaxis
```
set domain=<DomainName>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                                           Descripción                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Especifica un nombre nuevo para el nombre de dominio DNS predeterminado. El nombre de dominio predeterminado es el nombre de host. |
| {help &#124; ?} |                      Muestra un resumen breve de **nslookup** subcomandos.                      |

## <a name="remarks"></a>Comentarios
- El nombre de dominio DNS predeterminado se anexa a una solicitud de búsqueda según el estado de la **defname** y **búsqueda** opciones. La lista de búsqueda de dominio DNS contiene a los elementos primarios del dominio DNS predeterminado si tiene al menos dos componentes en su nombre. Por ejemplo, si el dominio DNS predeterminado es mfg.widgets.com, la lista de búsqueda se denomina mfg.widgets.com y widgets.com. Use la **establecer srchlist** comando para especificar una lista distinta y el **establecer todos los** comando para mostrar la lista.
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  [nslookup establece srchlist](nslookup-set-srchlist.md)
  [nslookup establece todas](nslookup-set-all.md)
