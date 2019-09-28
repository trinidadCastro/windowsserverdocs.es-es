---
title: bootcfg
description: 'Temas de comandos de Windows para **bootcfg** : configura, consulta o cambia la configuración del archivo boot. ini.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d66296327a2221093e5434f69e15e7c55df1f6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379854"
---
# <a name="bootcfg"></a>bootcfg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura, consulta o cambia la configuración del archivo Boot.ini.  
## <a name="syntax"></a>Sintaxis  
```  
bootcfg <parameter> [arguments...]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|[bootcfg addsw](bootcfg-addsw.md)|agrega opciones de carga del sistema operativo para una entrada específica del sistema operativo.|  
|[bootcfg copy](bootcfg-copy.md)|Realiza una copia de una entrada de arranque existente, a la que puede agregar opciones de la línea de comandos.|  
|[bootcfg dbg1394](bootcfg-dbg1394.md)|Configura la depuración de Puerto 1394 para una entrada de sistema operativo especificada.|  
|[bootcfg debug](bootcfg-debug.md)|agrega o cambia la configuración de depuración de una entrada de sistema operativo especificada.|  
|[bootcfg default](bootcfg-default.md)|Especifica la entrada del sistema operativo que se designará como predeterminada.|  
|[bootcfg delete](bootcfg-delete.md)|elimina una entrada del sistema operativo en la sección **[operating systems]** del archivo boot. ini.|  
|[bootcfg ems](bootcfg-ems.md)|Permite al usuario agregar o cambiar la configuración para la redirección de la consola de servicios de administración de emergencia a un equipo remoto.|  
|[bootcfg query](bootcfg-query.md)|Consulta y muestra las entradas de la sección [cargador de arranque] y **[sistemas operativos]** de boot. ini.|  
|[bootcfg raw](bootcfg-raw.md)|agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada del sistema operativo en la sección **[operating systems]** del archivo boot. ini.|  
|[bootcfg rmsw](bootcfg-rmsw.md)|quita las opciones de carga del sistema operativo para una entrada de sistema operativo especificada.|  
|[bootcfg timeout](bootcfg-timeout.md)|cambia el valor de tiempo de espera del sistema operativo.|  
