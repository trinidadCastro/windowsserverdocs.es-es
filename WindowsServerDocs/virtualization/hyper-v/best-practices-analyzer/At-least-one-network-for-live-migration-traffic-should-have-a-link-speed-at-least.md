---
title: Al menos una red para el tráfico de migración en vivo debe tener una velocidad de vínculo de al menos 1 Gbps
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ef75f73bd934b863b146e93f4cdc7323e5d4c6fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837736"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Al menos una red para el tráfico de migración en vivo debe tener una velocidad de vínculo de al menos 1 Gbps

>Se aplica a: Windows Server 2016


  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Ninguna de las redes para el tráfico de migración en vivo tienen una velocidad de vínculo de al menos 1 Gbps.*  
  
## <a name="impact"></a>Impacto  
*Las migraciones en vivo pueden producirse lentamente, lo que podría interrumpir la conexión de red debido a un tiempo de espera de conexión TCP.*  
  
## <a name="resolution"></a>Resolución  
*Configure al menos una red de migración en vivo con una velocidad de 1 Gbps o más.*  
  
Consulte la documentación de su proveedor de hardware de red para averiguar si cualquiera de los adaptadores de red existente puede admitir una velocidad de vínculo de al menos 1 Gbps.  
  


