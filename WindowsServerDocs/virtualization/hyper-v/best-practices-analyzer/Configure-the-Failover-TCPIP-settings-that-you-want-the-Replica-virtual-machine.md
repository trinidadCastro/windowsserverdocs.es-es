---
title: Configure las opciones de conmutación por error TCP/IP que desea que la máquina virtual de réplica para usar si se produce una conmutación por error
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c16fbc95c9d679611d57327992a6621d58d4e201
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855756"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Configure las opciones de conmutación por error TCP/IP que desea que la máquina virtual de réplica para usar si se produce una conmutación por error

>Se aplica a: Windows Server 2016
 
Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>Problema  
*Máquinas virtuales de réplica configuradas con una dirección IP estática debe configurarse para usar una dirección IP diferente de su homólogo de máquina virtual principal en el caso de conmutación por error.*  
  
## <a name="impact"></a>Impacto  
*Los clientes que usan la carga de trabajo compatible con la máquina virtual principal no es posible que pueda conectarse a la máquina virtual de réplica tras una conmutación por error. Además, dirección IP original de la máquina virtual principal no serán válida en la topología de red de máquina virtual de réplica. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use el Administrador de Hyper-V para configurar la dirección IP que debe usar la máquina virtual de réplica en el caso de conmutación por error.*  
  


