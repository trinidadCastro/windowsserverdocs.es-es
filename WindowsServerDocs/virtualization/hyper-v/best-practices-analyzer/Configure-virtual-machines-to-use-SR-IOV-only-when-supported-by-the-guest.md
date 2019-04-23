---
title: Configurar máquinas virtuales para utilizar SR-IOV solo si es compatible con el sistema operativo invitado
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c2acb21fe8b11e8f020c6d2ab1742116c23b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833366"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurar máquinas virtuales para utilizar SR-IOV solo si es compatible con el sistema operativo invitado

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
*Una o más máquinas virtuales están configuradas para usar virtualización de E/S de raíz única (SR-IOV), pero el sistema operativo invitado no admite SR-IOV*  
  
## <a name="impact"></a>Impacto  
*Funciones virtuales SR-IOV no se asignará a las máquinas virtuales siguientes:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Deshabilite SR-IOV en todas las máquinas virtuales que ejecutan sistemas operativos invitados que no son compatibles con SR-IOV.*  
  
SR-IOV solo se admite en algunos invitados de Windows de 64 bits. Para obtener más información, consulte [compatibilidad de características de Hyper-V mediante la generación e invitado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


