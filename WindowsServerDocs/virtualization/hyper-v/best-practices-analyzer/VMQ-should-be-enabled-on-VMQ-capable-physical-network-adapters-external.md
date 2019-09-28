---
title: VMQ debe estar habilitado en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e8607ee891ef693d4e4e7a868540237855aebd2e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393290"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>VMQ debe estar habilitado en los adaptadores de red físicos compatibles con VMQ enlazados a un conmutador virtual externo

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Los siguientes adaptadores de red son capaces de Virtual Machine Queue (VMQ) pero la funcionalidad está deshabilitada.*  
  
## <a name="impact"></a>**Impacto**  
*Windows no puede sacar el máximo partido de las descargas de hardware disponibles en los siguientes adaptadores de red:*  
  
\<list de los adaptadores de red >  
  
## <a name="resolution"></a>**Resolución**  
*Habilite VMQ con el cmdlet enable-NetAdapterVmq de Windows PowerShell o con la interfaz de usuario de propiedades avanzadas del adaptador de red.*  
  


