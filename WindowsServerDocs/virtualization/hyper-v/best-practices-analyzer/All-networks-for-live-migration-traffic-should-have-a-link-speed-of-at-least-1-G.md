---
title: Todas las redes para el tráfico de migración en vivo deben tener una velocidad de vínculo de al menos 1 Gbps
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: adb16b1c4618e0874f48f4715440a9d903f5bc9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857848"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Todas las redes para el tráfico de migración en vivo deben tener una velocidad de vínculo de al menos 1 Gbps

>Se aplica a: Windows Server 2016


  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Ninguna de las redes para el tráfico de migración en vivo tiene una velocidad de vínculo de al menos 1 Gbps.*  
  
## <a name="impact"></a>Impacto  
*Las migraciones en vivo pueden producirse lentamente, lo que podría interrumpir la conexión de red debido a un tiempo de espera de conexión TCP.*  
  
## <a name="resolution"></a>Resolución  
*Configure al menos una red de migración en vivo con una velocidad de 1 Gbps o más.*  
  
Consulte la documentación del proveedor de hardware de red para averiguar si alguno de los adaptadores de red existentes puede admitir una velocidad de vínculo de al menos 1 Gbps.  
  


