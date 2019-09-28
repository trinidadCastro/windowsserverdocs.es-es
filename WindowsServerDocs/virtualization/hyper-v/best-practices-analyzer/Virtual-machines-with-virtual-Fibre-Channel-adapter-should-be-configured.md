---
title: Las máquinas virtuales configuradas con un adaptador de Canal de fibra virtual deben configurarse para alta disponibilidad en el almacenamiento basado en Canal de fibra
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4c50ac70b51ab6a2e5cb8247b309070d85932a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364561"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Las máquinas virtuales configuradas con un adaptador de Canal de fibra virtual deben configurarse para alta disponibilidad en el almacenamiento basado en Canal de fibra

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Información de|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.
  
## <a name="issue"></a>**Problema**  
*Una o varias máquinas virtuales no tienen una conexión de alta disponibilidad con el almacenamiento basado en Canal de fibra porque esas máquinas virtuales están configuradas con un adaptador de Canal de fibra virtual que está conectado a un único adaptador de bus host (HBA).*  
  
## <a name="impact"></a>**Impacto**  
*A un error del adaptador de bus host podría bloquear la conexión Canal de fibra entre el almacenamiento y las máquinas virtuales. Esto afecta a las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Agregue otra conexión desde la máquina virtual al adaptador de bus host y configure e/s de múltiples rutas (MPIO) en el sistema operativo invitado para establecer conexiones Canal de fibra redundantes.*  
  


