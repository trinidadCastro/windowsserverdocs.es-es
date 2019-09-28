---
title: Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c9363fbce35552a8f7d279662ae9072bcd7ea480
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364825"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Asegúrese de que todas las extensiones de conmutador virtual obligatoria están disponibles

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Uno o más adaptadores de red virtual están conectados a un conmutador virtual con extensiones obligatorias que están deshabilitadas o no instaladas.*  
  
## <a name="impact"></a>Impacto  
*El tráfico de red está bloqueado en uno o varios adaptadores de red virtual de las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*First, asegúrese de que se ha instalado la extensión obligatoria en el host e instale la extensión si es necesario. A continuación, si la extensión obligatoria está deshabilitada, use el administrador de conmutadores virtuales o el cmdlet enable-VMSwitchExtension de Windows PowerShell para habilitar la extensión.*  
  


