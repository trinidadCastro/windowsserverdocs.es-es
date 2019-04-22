---
title: Una SAN virtual debe estar asociada a un adaptador de bus host físico
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3b9ca1e2da1cf9f4410f465fe95c6cc9c0b07ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819086"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Una SAN virtual debe estar asociada a un adaptador de bus host físico

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*Se ha configurado una red de área de almacenamiento virtual (SAN) sin una asociación a un adaptador de bus host (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Se producirá un error en una máquina virtual iniciar cuando se configura con un adaptador de canal de fibra virtual conectado a una SAN virtual mal configurada. Esto afecta a la siguiente SANs virtual:*  
  
  
\<lista de redes SAN virtuales >  
  
  
## <a name="resolution"></a>**Resolución**  
*Volver a configurar la SAN virtual conectándose a un adaptador de bus host.*  
  
  
  


