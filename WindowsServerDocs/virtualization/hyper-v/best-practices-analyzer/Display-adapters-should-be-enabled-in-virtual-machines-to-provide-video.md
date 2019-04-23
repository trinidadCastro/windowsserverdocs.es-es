---
title: Adaptadores de pantalla deben habilitarse en máquinas virtuales para proporcionar capacidades de vídeo
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d61461db471a876ddf46c1e5fec6992ffa80373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870696"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Adaptadores de pantalla deben habilitarse en máquinas virtuales para proporcionar capacidades de vídeo

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*El dispositivo de vídeo de Bus de máquina Virtual de Microsoft se puede deshabilitar en una máquina virtual.*  
  
Dispositivo de vídeo de Bus de máquina Virtual de Microsoft es un adaptador de vídeo virtual optimizado para usarse con máquinas virtuales de Hyper-V. Cuando una máquina virtual no está configurada para usar el dispositivo de vídeo de Bus de máquina Virtual de Microsoft, se usa un adaptador de vídeo antiguo. Dispositivo de vídeo de Bus de máquina Virtual de Microsoft se comporta mejor que un adaptador de vídeo antiguo.  
  
## <a name="impact"></a>Impacto  
  
*Se degradará el rendimiento de vídeo para las máquinas virtuales siguientes:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use el Administrador de dispositivos en el sistema operativo invitado para habilitar el dispositivo de vídeo de Bus de máquina Virtual de Microsoft.*  
  
Los pasos necesarios para usar el Administrador de dispositivos varían según el sistema operativo. Para obtener instrucciones, consulte la Ayuda en el sistema operativo invitado.  
  


