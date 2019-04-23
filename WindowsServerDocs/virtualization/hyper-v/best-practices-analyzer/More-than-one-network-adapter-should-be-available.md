---
title: Más de un adaptador de red debe estar disponible
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 678c0161e97b8dd022bbf0037d9add5de0281f77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884606"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Más de un adaptador de red debe estar disponible

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  

En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.

## <a name="issue"></a>Problema  
  
*Este servidor está configurado con un adaptador de red, debe compartirse por el sistema operativo de administración y todas las máquinas virtuales que requieren acceso a una red física.*  
  
## <a name="impact"></a>Impacto  
  
*Rendimiento de redes puede verse degradado en el sistema operativo de administración.*  
  
## <a name="resolution"></a>Resolución  
  
*Agregar más adaptadores de red a este equipo. Para reservar un adaptador de red para uso exclusivo por el sistema operativo de administración, no se configura para su uso con una red virtual externa.*  
  
Para obtener información acerca de cómo agregar un adaptador de red en el equipo, consulte la documentación del equipo o el adaptador de red. A continuación, para reservarlo exclusivamente para el sistema operativo de administración, no conectarla a un conmutador virtual.   
  


