---
title: Todos los adaptadores de red virtual deben estar habilitados
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a769c3203f6c6946f01cd91b66fbec38af83bbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837136"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Todos los adaptadores de red virtual deben estar habilitados

>Se aplica a: Windows Server 2016


  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o más adaptadores de red virtual asociados con un adaptador de red físicos están deshabilitados en el sistema operativo de administración.*  
  
## <a name="impact"></a>Impacto  
  
*La configuración de este servidor no es óptima.*  
  
El sistema operativo de administración no se puede conectar a una red (externa) física mediante uno de los adaptadores de red físico en este equipo porque tiene asociados con un adaptador de red virtual deshabilitados.  
  
## <a name="resolution"></a>Resolución  
  
*Use la configuración de Internet y red para habilitar al adaptador de red virtual. También puede usar el Administrador de conmutadores virtuales para volver a configurar el conmutador virtual externo para que no se comparte con el sistema operativo de administración.*  
  


