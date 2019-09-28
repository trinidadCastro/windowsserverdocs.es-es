---
title: Una SAN virtual debe estar asociada a un adaptador de bus host físico
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e86f8d9b9a4a87fd6457954c3a4723857faac3b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366690"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Una SAN virtual debe estar asociada a un adaptador de bus host físico

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
*Se ha configurado una red de área de almacenamiento virtual (SAN) sin una asociación a un adaptador de bus host (HBA).*  
  
## <a name="impact"></a>**Impacto**  
la máquina virtual *A no se iniciará cuando esté configurada con un adaptador de Canal de fibra virtual conectado a una SAN Virtual mal configurada. Esto afecta a las siguientes San virtuales:*  
  
  
\<list de San virtuales >  
  
  
## <a name="resolution"></a>**Resolución**  
*Vuelva a configurar la SAN Virtual conectándola a un adaptador de bus host.*  
  
  
  


