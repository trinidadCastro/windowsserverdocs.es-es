---
title: Asegúrese de que el controlador de función virtual funciona correctamente cuando una máquina virtual está configurada para utilizar SR-IOV
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d3d0a5008b55d4823cef9a8dd2a7bce4a6a2a33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852086"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Asegúrese de que el controlador de función virtual funciona correctamente cuando una máquina virtual está configurada para utilizar SR-IOV

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*El controlador de función virtual no está funcionando correctamente en el sistema operativo invitado de una o más máquinas virtuales.*  
  
## <a name="impact"></a>Impacto  
*Rendimiento de las redes no es óptimo en las máquinas virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*En el sistema operativo invitado, realice lo siguiente: Compruebe que están instalados los controladores adecuados y todos los dispositivos de red están habilitados y comprobación el registro de eventos para errores o advertencias.*  
  


