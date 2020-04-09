---
title: help
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 404cefe879e8f63678f2cba90516fad9c216eb23
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842318"
---
# <a name="help"></a>help

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
help [<command>]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro |                              Descripción                              |
|-----------|-----------------------------------------------------------------------|
| <command> | Especifica el comando para el que se va a mostrar información de ayuda detallada. |
  
## <a name="remarks"></a>Comentarios  
  
-   Si no se especifica ningún comando, la **ayuda** mostrará todos los comandos posibles.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example  
Para mostrar una lista de todos los comandos disponibles en DiskPart, escriba:  
  
```  
help  
```  
  
Para mostrar información detallada sobre la ayuda sobre cómo usar el comando **Create Partition Primary** en DiskPart, escriba:  
  
```  
help create partition primary  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

  

