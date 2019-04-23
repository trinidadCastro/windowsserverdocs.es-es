---
title: Se recomienda la autenticación basada en certificados para la replicación
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5bd131b48b009b43b6379f72f370b4179dea5a03
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873066"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>Se recomienda la autenticación basada en certificados para la replicación

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
*Una o varias máquinas virtuales seleccionadas para la replicación se configuran para la autenticación Kerberos.*  
  
## <a name="impact"></a>**Impact**  
*No se cifra el tráfico de red de replicación desde el servidor principal al servidor de replicación. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Si está usando otro método para realizar el cifrado, puede omitir esto. En caso contrario, modifique la configuración de máquina virtual para elegir la autenticación basada en certificados.*  
  


