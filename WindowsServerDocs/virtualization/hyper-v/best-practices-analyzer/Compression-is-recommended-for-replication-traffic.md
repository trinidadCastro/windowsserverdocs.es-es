---
title: Se recomienda la compresión para el tráfico de replicación
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 77a314e816c36f626ea3edb10b80f65e3897e7c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365100"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>Se recomienda la compresión para el tráfico de replicación

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
*Se descomprimirá el tráfico de replicación enviado a través de la red desde el servidor principal al servidor réplica.*  
  
## <a name="impact"></a>Impacto  
el tráfico *Replication usará más ancho de banda de lo necesario. Esto afecta a las siguientes máquinas virtuales:*  
  
\<list de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Configure réplica de Hyper-V para comprimir los datos transmitidos a través de la red en la configuración de la máquina virtual en el administrador de Hyper-V. También puede usar herramientas fuera de Hyper-V para realizar la compresión.*  
  


