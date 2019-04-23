---
title: lodctr
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ea48067bd78d260824da7d091f94957b51b768d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852226"
---
# <a name="lodctr"></a>lodctr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que se registre o guardar la configuración de nombre y del registro de contador de rendimiento en un archivo y designar los servicios de confianza.
## <a name="syntax"></a>Sintaxis
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<filename>|Registra la configuración de nombre de contador de rendimiento y el texto explicativo proporcionado en el archivo de inicialización del nombre de archivo.|
|/s:<filename>|Rendimiento guarda valores del registro de contador y texto explicativo al archivo <filename>.|
|/r|Restaura la configuración del registro de contador y texto explicativo de la configuración actual del registro y archivos almacenados en caché de rendimiento relacionados con el registro.<br /><br />Esta opción solo está disponible en el sistema operativo Windows Server 2003.|
|/r:<filename>|Contador de configuración del registro de rendimiento de restauraciones y texto explicativo de archivo <filename>. **Advertencia:** si usas el **lodctr / r** comando, se sobrescribirá todas las configuraciones del registro de contador de rendimiento y el texto explicativo reemplazarlos con la configuración definida en el archivo especificado.|
|/t:<servicename>|Indica que el servicio <servicename> es de confianza.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
Si la información que se proporciona contiene espacios, utilice comillas alrededor del texto (por ejemplo, "<filename>").
## <a name="BKMK_Examples"></a>Ejemplos
Para guardar la configuración del registro de rendimiento actual y texto explicativo al archivo de contador **backup1.txt perf**:
```
lodctr /s:"perf backup1.txt"
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

