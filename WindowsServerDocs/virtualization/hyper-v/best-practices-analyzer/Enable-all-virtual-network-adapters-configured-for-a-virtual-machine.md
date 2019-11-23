---
title: Habilitar todos los adaptadores de red virtuales configurados para una máquina virtual
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bdca25be4af41d0f6ddfafe885f8c2b1301b71fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393648"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Habilitar todos los adaptadores de red virtuales configurados para una máquina virtual

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Es posible que uno o más adaptadores de red estén deshabilitados en una máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*Es posible que las siguientes máquinas virtuales no tengan conectividad de red:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use Device Manager del sistema operativo invitado para habilitar todos los adaptadores de red virtuales. Si el adaptador no es necesario, use el administrador de Hyper-V para quitarlo de la máquina virtual.*  
  


