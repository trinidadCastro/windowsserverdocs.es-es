---
title: Las conmutaciones por error de prueba deben llevarse a cabo al menos cada mes para comprobar que la conmutación por error se realizará correctamente y que las cargas de trabajo de máquina virtual funcionará como se esperaba después de la conmutación por error
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 879c860caede942393f0929faab9e4d567f225a6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856146"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>Las conmutaciones por error de prueba deben llevarse a cabo al menos cada mes para comprobar que la conmutación por error se realizará correctamente y que las cargas de trabajo de máquina virtual funcionará como se esperaba después de la conmutación por error

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*No ha habido ninguna conmutación por error de prueba en al menos un mes.*  
  
## <a name="impact"></a>Impacto  
*No hay ninguna confirmación de que se realizará correctamente una conmutación por error planeada o no planeada o las operaciones de carga de trabajo continúen funcionando correctamente después de una conmutación por error. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el Administrador de Hyper-V para llevar a cabo una conmutación por error de prueba.*  
  


