---
title: ayuda
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3d76a71ec287e5c874ae3e4dec34016c5c80336
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375582"
---
# <a name="help"></a>ayuda

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
help [<command>]  
```  
  
## <a name="parameters"></a>Parámetros  
  
| Parámetro |                              Descripción                              |
|-----------|-----------------------------------------------------------------------|
| <command> | Especifica el comando para el que se va a mostrar información de ayuda detallada. |
  
## <a name="remarks"></a>Comentarios  
  
-   Si no se especifica ningún comando, la **ayuda** mostrará todos los comandos posibles.  
  
## <a name="BKMK_examples"></a>Example  
Para mostrar una lista de todos los comandos disponibles en DiskPart, escriba:  
  
```  
help  
```  
  
Para mostrar información detallada sobre la ayuda sobre cómo usar el comando **Create Partition Primary** en DiskPart, escriba:  
  
```  
help create partition primary  
```  
  
#### <a name="additional-references"></a>Referencias adicionales  
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

