---
title: La interfaz de equipo enlazada a un conmutador virtual debe estar en el modo predeterminado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fe19de5dd380d08c01c917da9d4e2ef9465de042
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854608"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>La interfaz de equipo enlazada a un conmutador virtual debe estar en el modo predeterminado

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Algunos conmutadores virtuales están enlazados a una interfaz de equipo, pero la interfaz de equipo no pasa el tráfico de todas las VLAN a los conmutadores virtuales.*  
  
## <a name="impact"></a>**Impacto**  
*Los siguientes conmutadores virtuales no pueden tener acceso a todas las VLAN: \n{0}*  
  
## <a name="resolution"></a>**Resolución**  
*Use Administrador del servidor o el cmdlet de Windows PowerShell Set-NetLbfoTeamNic para restablecer la interfaz de equipo en el modo predeterminado.*  
  


