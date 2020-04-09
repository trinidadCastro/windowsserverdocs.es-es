---
title: Hay más de un adaptador de red disponible
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 56cb747ac44d48b115dbf105ea96e4623d458b28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861898"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Hay más de un adaptador de red disponible

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema  
  
*Este servidor se configura con un adaptador de red, que debe ser compartido por el sistema operativo de administración y todas las máquinas virtuales que requieran acceso a una red física.*  
  
## <a name="impact"></a>Impacto  
  
*El rendimiento de red puede verse afectado en el sistema operativo de administración.*  
  
## <a name="resolution"></a>Resolución  
  
*Agregue más adaptadores de red a este equipo. Para reservar un adaptador de red para uso exclusivo del sistema operativo de administración, no lo configure para su uso con una red virtual externa.*  
  
Para obtener información acerca de cómo agregar un adaptador de red al equipo, consulte la documentación del equipo o del adaptador de red. A continuación, para reservarlo exclusivamente para el sistema operativo de administración, no lo conecte a un conmutador virtual.   
  


