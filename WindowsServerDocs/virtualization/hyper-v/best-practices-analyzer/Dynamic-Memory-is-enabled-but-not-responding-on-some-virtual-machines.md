---
title: Memoria dinámica está habilitada pero no responde en algunas máquinas virtuales
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 95fd426929f3e2f6f01bc10b207a21a57f1d8370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887786"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>Memoria dinámica está habilitada pero no responde en algunas máquinas virtuales

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
*Una o más máquinas virtuales están teniendo problemas con el controlador necesario para la memoria dinámica en el sistema operativo invitado.*  
  
## <a name="impact"></a>Impacto  
*El sistema operativo invitado en las siguientes máquinas virtuales podrían no ejecutarse o podría ejecutarse no confiable porque Hyper-V no puede ajustar la memoria dinámicamente para responder a los cambios en la demanda de memoria. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Este es el comportamiento esperado si se está iniciando la máquina virtual. Si no se está iniciando la máquina virtual, asegúrese de que los servicios de integración están actualizados con la versión más reciente y que el sistema operativo invitado es compatible con memoria dinámica.*  
  
A partir de Windows Server 2016, los servicios de integración se entregan a través de Windows Update. Asegúrese de que las máquinas virtuales están configuradas para recibir actualizaciones para obtener la versión más reciente de integration services.  
  
Memoria dinámica funciona con versiones específicas de los invitados compatibles. Consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx) para las versiones anteriores a Windows Server 2016 y Windows 10.  
  


