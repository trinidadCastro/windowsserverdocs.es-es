---
title: Un equipo enlazado a un conmutador virtual solo debe tener una interfaz de equipo expuesta
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6baa9e4ae900c9b671003872b4eb4589efb2f085
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365404"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Un equipo enlazado a un conmutador virtual solo debe tener una interfaz de equipo expuesta

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
*Uno o varios conmutadores virtuales están enlazados a un equipo que tiene varias interfaces de equipo.*  
  
## <a name="impact"></a>**Impacto**  
*Es posible que los siguientes conmutadores virtuales no tengan acceso a las VLAN y al ancho de banda usado por otras interfaces de equipo:*  
  
\<list de los conmutadores virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Use el cmdlet de Windows PowerShell Remove-NetLbfoTeamNic para quitar todas las interfaces de equipo del equipo que no sean la interfaz de equipo predeterminada.*  
  


