---
title: No se deben configurar los puertos serie en máquinas virtuales de generación 2
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58c3fc5f975b85ce17ac5f7cca4930ec9e851e07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877386"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>No se deben configurar los puertos serie en máquinas virtuales de generación 2

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
*Generación de uno o más 2 máquinas virtuales tiene un puerto serie configurado.*  
  
## <a name="impact"></a>**Impact**  
*Puede verse afectado el rendimiento de las máquinas virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Si esto es intencionado, se requiere ninguna acción adicional. En caso contrario, considere el uso de administrador de Hyper-V o Windows PowerShell para quitar la cadena de conexión de los puertos serie en la máquina virtual.*  
  


