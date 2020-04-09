---
title: unlodctr
description: Tema de comandos de Windows para Unlodctr, que quita nombres de contadores de rendimiento y texto explicativo de un servicio o controlador de dispositivo del registro del sistema
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe7fc3c9eafefd59a5daab625e3af06b6addd292
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832262"
---
# <a name="unlodctr"></a>unlodctr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita los nombres de los **contadores de rendimiento** y el texto **explicativo** de un servicio o controlador de dispositivo del registro del sistema.   

## <a name="syntax"></a>Sintaxis  
```  
Unlodctr <DriverName>   
```  
#### <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|\<DriverName >|quita la configuración del nombre del contador de rendimiento y el texto explicativo de los <DriverName> de controlador o servicio del registro de Windows Server 2003.|  
|/?|Muestra la Ayuda en el símbolo del sistema.|  

## <a name="remarks"></a>Comentarios  
> [!WARNING]  
> Una modificación incorrecta del Registro puede provocar daños graves en el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.  

Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, <DriverName>).  

## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Para quitar la configuración del registro de rendimiento actual y el texto de explicación del contador para el servicio de Protocolo simple de transferencia de correo (SMTP):  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
