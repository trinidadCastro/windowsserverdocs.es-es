---
title: Los servicios de integración deben instalarse antes de que las máquinas virtuales principales o de réplica puedan usar una dirección IP alternativa después de una conmutación por error.
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados, con vínculos a más información.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
ms.date: 8/16/2016
ms.openlocfilehash: 3a612c9e119ac2b74bea070feb458703dd50040f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745830"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Los servicios de integración deben instalarse antes de que las máquinas virtuales principales o de réplica puedan usar una dirección IP alternativa después de una conmutación por error.

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Las máquinas virtuales que participan en la replicación se pueden configurar para usar una dirección IP específica en caso de conmutación por error, pero solo si Integration Services está instalado en el sistema operativo invitado de la máquina virtual.*

## <a name="impact"></a>Impacto
*En el caso de una conmutación por error (planeada, no planeada o de prueba), la máquina virtual de réplica se conectará con la misma dirección IP que la máquina virtual principal. Esta configuración puede provocar problemas de conectividad. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Use conexión a máquina virtual para instalar los servicios de integración en la máquina virtual.*

A partir de Windows Server 2016, los servicios de integración para máquinas virtuales Windows se entregan a través de Windows Update. Asegúrese de que estas máquinas virtuales están configuradas para recibir actualizaciones de Windows para obtener la versión más reciente de Integration Services. El kernel de Linux incluye ahora Linux Integration Services (LIS) y se actualiza para las nuevas versiones, pero las distribuciones de Linux basadas en kernels anteriores pueden no tener las últimas mejoras o correcciones. Para obtener más información, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


