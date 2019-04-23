---
title: Configurar los sistemas operativos invitados para copias de seguridad basadas en VSS habilitar las instantáneas coherentes con la aplicación para la réplica de Hyper-V
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4300dd4b7adc0cef8544215b5da62044a97301b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863896"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurar los sistemas operativos invitados para copias de seguridad basadas en VSS habilitar las instantáneas coherentes con la aplicación para la réplica de Hyper-V

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
*Las instantáneas coherentes con la aplicación requiere que los servicios de instantáneas de volumen (VSS) está habilitado y configurado en los sistemas operativos invitados de máquinas virtuales que participan en la replicación.*  
  
## <a name="impact"></a>Impacto  
*Incluso si las instantáneas coherentes con la aplicación se especifican en la configuración de replicación, Hyper-V no usará ellas a menos que se configura el VSS. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Usar conexión a máquina Virtual para instalar integration services en la máquina virtual.*  
  


