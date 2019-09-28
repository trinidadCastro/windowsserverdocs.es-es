---
title: Uno o más adaptadores de red deben configurarse como origen para la creación de reflejo del puerto
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a079033608b8af8c63a0d02ae166eb7280fec41d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393567"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Uno o más adaptadores de red deben configurarse como origen para la creación de reflejo del puerto

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
*Una o varias máquinas virtuales tienen un adaptador de red configurado como destino para la creación de reflejo del puerto, pero no hay ningún origen correspondiente en el conmutador virtual.*  
  
## <a name="impact"></a>**Impacto**  
*La creación de reflejo del puerto no funcionará correctamente para los siguientes conmutadores virtuales y máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use Windows PowerShell o el administrador de Hyper-V para completar o corregir la configuración de creación de reflejo del puerto.*  
  


