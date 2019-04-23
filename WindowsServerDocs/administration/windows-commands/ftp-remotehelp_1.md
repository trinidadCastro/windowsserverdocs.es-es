---
title: ftp remotehelp_1
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd64af157f7ce05330cdafe6e4db6787fa765859
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889596"
---
# <a name="ftp-remotehelp1"></a>ftp: remotehelp_1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra ayuda para los comandos remotos.   
## <a name="syntax"></a>Sintaxis  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[<Command>]|Especifica el nombre del comando sobre el que desea obtener ayuda. Si *comando* no se especifica, **ftp** muestra una lista de todos los comandos remotos.|  
## <a name="remarks"></a>Comentarios  
Puede ejecutar comandos remotos con **oferta** o **literal**.  
## <a name="BKMK_Examples"></a>Ejemplos  
Mostrar una lista de comandos remotos.  
```  
remotehelp  
```  
Mostrar la sintaxis de la **Func** comando remoto.  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [ftp: quote](ftp-quote.md)  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
