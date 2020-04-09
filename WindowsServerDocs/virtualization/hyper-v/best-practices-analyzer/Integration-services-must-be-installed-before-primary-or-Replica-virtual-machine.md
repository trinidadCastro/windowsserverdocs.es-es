---
title: Los servicios de integración deben instalarse antes de que las máquinas virtuales principales o de réplica puedan usar una dirección IP alternativa después de una conmutación por error.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados, con vínculos a más información.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d532d58d21b39963b41de969d83b720ed077e9c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861908"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Los servicios de integración deben instalarse antes de que las máquinas virtuales principales o de réplica puedan usar una dirección IP alternativa después de una conmutación por error.

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*Las máquinas virtuales que participan en la replicación se pueden configurar para usar una dirección IP específica en caso de conmutación por error, pero solo si Integration Services está instalado en el sistema operativo invitado de la máquina virtual.*  
  
## <a name="impact"></a>Impacto  
*En el caso de una conmutación por error (planeada, no planeada o de prueba), la máquina virtual de réplica se conectará con la misma dirección IP que la máquina virtual principal. Esta configuración puede provocar problemas de conectividad. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Use conexión a máquina virtual para instalar los servicios de integración en la máquina virtual.*  
  
A partir de Windows Server 2016, los servicios de integración para máquinas virtuales Windows se entregan a través de Windows Update. Asegúrese de que estas máquinas virtuales están configuradas para recibir actualizaciones de Windows para obtener la versión más reciente de Integration Services. El kernel de Linux incluye ahora Linux Integration Services (LIS) y se actualiza para las nuevas versiones, pero las distribuciones de Linux basadas en kernels anteriores pueden no tener las últimas mejoras o correcciones. Para obtener más información, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


