---
title: unlodctr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e1a662da10acc65b4ad2fd0d055cf9d46de603be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886656"
---
# <a name="unlodctr"></a>unlodctr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita los nombres de contador de rendimiento y texto explicativo para un servicio o controlador de dispositivo del registro del sistema.   

## <a name="syntax"></a>Sintaxis  
```  
Unlodctr <DriverName>   
```  
### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|\<DriverName>|Quita el rendimiento de la configuración del nombre de contador y texto explicativo para controlador o servicio <DriverName> desde el registro de Windows Server 2003.|  
|/?|Muestra la ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
> [!WARNING]  
> La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.  

Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, "<DriverName>").  

## <a name="BKMK_Examples"></a>Ejemplos  
Para quitar la configuración del registro de rendimiento actual y texto explicativo para el servicio de Protocolo Simple de transferencia de correo (SMTP) de contador:  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
