---
title: Configuración de PVLAN en un conmutador virtual debe ser coherente
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b7d6d068027aa9497b00138bd1d889ea86aa3308
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813256"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>Configuración de PVLAN en un conmutador virtual debe ser coherente

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>**Problema**  
*Red de área Local de privada Virtual (PVLAN) no está configurado correctamente en uno o más adaptadores de red virtual.*  
  
## <a name="impact"></a>**Impact**  
*PVLAN no podría aislar el tráfico de red entre máquinas virtuales correctamente.*  
  
## <a name="resolution"></a>**Resolución**  
*Use el cmdlet de Windows PowerShell, Set-VMNetworkAdapterVlan, configurar PVLAN correctamente.*  
  


