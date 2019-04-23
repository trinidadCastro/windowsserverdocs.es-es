---
title: Discos duros virtuales con archivos de paginación que se deben excluir de la replicación
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8b6289a82c83f3dcfc0de299250ce19ee3782678
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850126"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Discos duros virtuales con archivos de paginación que se deben excluir de la replicación

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Información de|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Se deben excluir los archivos de paginación de participar en la replicación, pero no se encontraron discos se han excluido.*  
  
## <a name="impact"></a>Impacto  
*Archivos de paginación experimenten un gran volumen de actividad de entrada/salida, lo que requiere innecesariamente mucho mayor de recursos que participan en la replicación. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Si aún no lo ha hecho, cree un disco duro virtual independiente para el archivo de paginación de Windows. Si ya se ha completado la replicación inicial, use el Administrador de Hyper-V para quitar la replicación. A continuación, configurar la replicación de nuevo y excluir el disco duro virtual con el archivo de paginación de la replicación.*  
  


