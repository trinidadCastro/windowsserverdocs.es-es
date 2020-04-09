---
title: Las instantáneas de recuperación se deben quitar después de la conmutación por error
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c995293ca67b4cad0837affa854fb4ac366856e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861848"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Las instantáneas de recuperación se deben quitar después de la conmutación por error

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*Una máquina virtual conmutada por error tiene una o varias instantáneas de recuperación.*  
  
## <a name="impact"></a>**Impacto**  
*Se puede agotar el espacio disponible en el disco físico que almacena los archivos de instantáneas. Si esto ocurre, no se pueden realizar más operaciones de disco en el almacenamiento físico. Cualquier máquina virtual que se base en el almacenamiento físico podría verse afectada. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Para cada máquina virtual conmutada por error, use el cmdlet complete-VMFailover en Windows PowerShell para quitar las instantáneas de recuperación e indicar la finalización de la conmutación por error.*  
  


