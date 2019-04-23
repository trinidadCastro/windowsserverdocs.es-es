---
title: Uno o más adaptadores de red deben configurarse como el destino de la duplicación de puertos
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b83c166d-f010-47c4-a4bb-02167f2e3361
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e3fa986ca66e6da03797db4fe7183b9bae1fbdda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875736"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-destination-for-port-mirroring"></a>Uno o más adaptadores de red deben configurarse como el destino de la duplicación de puertos

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
*Una o más máquinas virtuales tienen un adaptador de red configurado como un origen para la creación de reflejo de puerto, pero no hay ningún destino correspondiente en el conmutador virtual.*  
  
## <a name="impact"></a>**Impact**  
*Creación de reflejo del puerto no funcionará correctamente para las máquinas virtuales y los conmutadores virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Usar Windows PowerShell o administrador de Hyper-V para completar o corregir la configuración de la duplicación de puertos.*  
  


