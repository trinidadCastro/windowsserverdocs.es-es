---
title: Resincronización de la replicación se debe programar horas de poca actividad
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2d6c18b7e37c5d17f56f41c7ff03ed8796457de0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840026"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>Resincronización de la replicación se debe programar horas de poca actividad

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
*Resincronización de la replicación para las máquinas virtuales principales no está programada para horas de poca actividad.*  
  
## <a name="impact"></a>Impacto  
*Más de una máquina virtual está en un estado que requieren resincronización, más los archivos de registro de replicación crecen y se producen los cambios no replicados más en las máquinas virtuales principales. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el Administrador de Hyper-V para modificar la configuración de replicación para la máquina virtual para llevar a cabo la resincronización automáticamente durante las horas de poca actividad.*  
  


