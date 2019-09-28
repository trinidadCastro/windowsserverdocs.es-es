---
title: La interfaz de equipo enlazada a un conmutador virtual debe estar en el modo predeterminado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9bfd0c98e865a0faae8dd70e97696e2c2682531b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393428"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>La interfaz de equipo enlazada a un conmutador virtual debe estar en el modo predeterminado

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
*Algunos conmutadores virtuales están enlazados a una interfaz de equipo, pero la interfaz de equipo no pasa el tráfico de todas las VLAN a los conmutadores virtuales.*  
  
## <a name="impact"></a>**Impacto**  
*Los siguientes conmutadores virtuales no pueden tener acceso a todas las VLAN: \n @ no__t-1*  
  
## <a name="resolution"></a>**Resolución**  
*Use Administrador del servidor o el cmdlet de Windows PowerShell Set-NetLbfoTeamNic para restablecer la interfaz de equipo en el modo predeterminado.*  
  


