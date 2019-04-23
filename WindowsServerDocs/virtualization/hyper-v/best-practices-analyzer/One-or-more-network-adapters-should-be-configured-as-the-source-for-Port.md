---
title: Uno o más adaptadores de red deben estar configurados como el origen para la creación de reflejo de puerto
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0843c1f302b96d334e8d6649ac5503bbe2b12d02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831796"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Uno o más adaptadores de red deben estar configurados como el origen para la creación de reflejo de puerto

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Una o más máquinas virtuales tienen un adaptador de red configurado como un destino para la creación de reflejo de puerto, pero no hay ningún origen correspondiente en el conmutador virtual.*  
  
## <a name="impact"></a>**Impact**  
*Creación de reflejo del puerto no funcionará correctamente para las máquinas virtuales y los conmutadores virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Usar Windows PowerShell o administrador de Hyper-V para completar o corregir la configuración de la duplicación de puertos.*  
  


