---
title: nslookup server
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e24e55026d12a0d8afc5b6f1bef926ece9087bd0
ms.sourcegitcommit: 6423dfa9cecb3b06bdd563cae113c3e80a4ec330
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105012"
---
# <a name="nslookup-server"></a>nslookup server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cambia el servidor predeterminado al dominio del sistema de nombres de dominio (DNS) especificado.
## <a name="syntax"></a>Sintaxis
```
server <DNSDomain>
```
## <a name="parameters"></a>Parámetros

|    Parámetro    |                          DESCRIPCIÓN                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Necesario. Especifica el nuevo dominio DNS para el servidor predeterminado. |
| {ayuda &#124; ?} |     Muestra un breve resumen de los subcomandos de **nslookup** .      |

## <a name="remarks"></a>Comentarios
- El comando **Server** usa el servidor predeterminado actual para buscar la información sobre el dominio DNS especificado. Esto contrasta con el comando **lserver** , que utiliza el servidor inicial.
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)[nslookup lserver](nslookup-lserver.md) 
  
