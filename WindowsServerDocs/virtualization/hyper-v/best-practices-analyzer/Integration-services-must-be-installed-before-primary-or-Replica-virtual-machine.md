---
title: Servicios de integración deben instalarse antes que la principal o máquinas virtuales de réplica puede usar una dirección IP alternativa después de una conmutación por error
description: Versión en línea del texto para esta regla de Best Practices Analyzer, con vínculos para obtener más información.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1ff8dbfd71655aee86ba7d0feac87ec2267a2171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865516"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Servicios de integración deben instalarse antes que la principal o máquinas virtuales de réplica puede usar una dirección IP alternativa después de una conmutación por error

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
*Las máquinas virtuales que participan en la replicación puede ser configurado para usar una dirección IP específica en el caso de conmutación por error, pero solo si están instalados los servicios de integración en el sistema operativo invitado de la máquina virtual.*  
  
## <a name="impact"></a>Impacto  
*En el caso de una conmutación por error (planeada, no planeada o de prueba), la máquina virtual de réplica se conectará de uso de la misma dirección IP como la máquina virtual principal. Esta configuración puede provocar problemas de conectividad. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Usar conexión a máquina Virtual para instalar integration services en la máquina virtual.*  
  
A partir de Windows Server 2016 integration services para las máquinas virtuales de Windows se entregan a través de Windows Update. Asegúrese de que estas máquinas virtuales están configuradas para recibir actualizaciones de Windows para obtener la versión más reciente de integration services. El kernel de Linux ahora incluye servicios de integración de Linux (LIS) y se actualiza para las nuevas versiones, pero las distribuciones de Linux en función de los kernels anteriores no pueden tener las últimas mejoras o correcciones. Para obtener más información, consulte [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


