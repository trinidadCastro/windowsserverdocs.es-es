---
title: help
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75dbf94f-d79c-45b2-9463-c06648218f4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6887edfd41895bb151c5aaebb88d8b72198f4dbf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724907"
---
# <a name="help"></a>help

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de los comandos disponibles o información de ayuda detallada sobre un comando especificado.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
help [<command>]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro |                              Descripción                              |
|-----------|-----------------------------------------------------------------------|
| <command> | Especifica el comando para el que se va a mostrar información de ayuda detallada. |
  
## <a name="remarks"></a>Observaciones  
  
-   Si no se especifica ningún comando, la **ayuda** mostrará todos los comandos posibles.  
  
## <a name="examples"></a>Ejemplos  
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
  

  

