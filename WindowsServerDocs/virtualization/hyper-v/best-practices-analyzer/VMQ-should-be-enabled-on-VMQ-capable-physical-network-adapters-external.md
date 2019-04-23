---
title: VMQ debe estar habilitada en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9a552c15675e6ca7a7310c8c9eaec883653987be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889476"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>VMQ debe estar habilitada en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo

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
*Los siguientes adaptadores de red son capaces de virtual machine queue (VMQ), pero la funcionalidad está deshabilitada.*  
  
## <a name="impact"></a>**Impact**  
*Windows no puede aprovechar al máximo las descargas de hardware disponible en los siguientes adaptadores de red:*  
  
\<lista de adaptadores de red >  
  
## <a name="resolution"></a>**Resolución**  
*Habilitar VMQ mediante el cmdlet Enable-NetAdapterVmq Windows PowerShell o mediante la interfaz de usuario de propiedades avanzadas del adaptador de red.*  
  


