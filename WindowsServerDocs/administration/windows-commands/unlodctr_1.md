---
title: unlodctr
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85a66b521f404358705962078f33af4bec1ebae5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363899"
---
# <a name="unlodctr"></a>unlodctr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita los nombres de los contadores de rendimiento y el texto explicativo de un servicio o controlador de dispositivo del registro del sistema.   

## <a name="syntax"></a>Sintaxis  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|@no__t 0DriverName >|quita la configuración del nombre del contador de rendimiento y el texto explicativo para el controlador o el servicio <DriverName> del registro de Windows Server 2003.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
> [!WARNING]  
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.  

Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, "<DriverName>").  

## <a name="BKMK_Examples"></a>Example  
Para quitar la configuración del registro de rendimiento actual y el texto de explicación del contador para el servicio de Protocolo simple de transferencia de correo (SMTP):  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
