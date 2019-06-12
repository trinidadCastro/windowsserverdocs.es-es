---
title: ¿Qué tipo de instalación es adecuado para usted
description: Este tema describe las distintas opciones de instalación para Windows Admin Center, incluida la instalación en un equipo con Windows 10 o un servidor de Windows para su uso por varios administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 9b26ce28d8b3f74c26adab87e68b9985f2be5361
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811818"
---
# <a name="what-type-of-installation-is-right-for-you"></a>¿Qué tipo de instalación es el adecuado para ti?

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Este tema describe las distintas opciones de instalación para Windows Admin Center, incluida la instalación en un equipo con Windows 10 o un servidor de Windows para su uso por varios administradores. Para instalar Windows Admin Center en una máquina virtual en Azure, consulte [implementar Windows Admin Center en Azure](../azure/deploy-wac-in-azure.md).

## <a name="supported-operating-systems-installation"></a>Sistemas operativos compatibles: Instalación

También puede **instalar** Windows Admin Center en los siguientes sistemas operativos de Windows:

| **Versión**  | **Modo de instalación** |
| -------------| -----------------------|
| Windows 10, versión 1709 o versiones más reciente | Modo de escritorio |
| Canal semianual de Windows Server | Modo de puerta de enlace |
| Windows Server 2016 | Modo de puerta de enlace |
| Windows Server 2019 | Modo de puerta de enlace |

**Modo de escritorio:** Iniciar desde el menú Inicio y conectarse a la puerta de enlace de Windows Admin Center desde el mismo equipo donde está instalado (es decir, `https://localhost:6516`)

**Modo de puerta de enlace:** Conectarse a la puerta de enlace de Windows Admin Center desde un explorador cliente en un equipo diferente (es decir, `https://servername.contoso.com`) 

> [!WARNING]
> No se admite la instalación de Windows Admin Center en un controlador de dominio. [Obtenga más información acerca de la seguridad de controladores de dominio procedimientos recomendados](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> No se puede usar Internet Explorer para administrar Windows Admin Center: en su lugar, deberá usar un [explorador compatible](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Si va a instalar Windows Admin Center en Windows Server, se recomienda administrar mediante la conexión remota con Windows 10 y borde.  Como alternativa, puede administrar localmente en Windows Server si ha instalado un explorador compatible.

## <a name="supported-operating-systems-management"></a>Sistemas operativos compatibles: Management

También puede **administrar** los siguientes sistemas operativos de Windows mediante Windows Admin Center:

| `Version` | Administrar *nodo* a través de *administrador del servidor* | Administrar *clúster* a través de *Administrador de clústeres de conmutación por error* | Administrar *HCI* a través de *HCI el Administrador de clústeres* |
| ------------------------- |--------------- | ----- | ------------------------ |
| Windows 10, versión 1709 o versiones más reciente | Sí (a través de administración de equipos) | N/D | N/D |
| Canal semianual de Windows Server | Sí | Sí | N/D |
| Windows Server 2019 | Sí | Sí | Sí |
| Windows Server 2016 | Sí | Sí | Sí, con [actualización acumulativa más reciente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sí | Sí | N/D |
| Windows Server 2012 R2 | Sí | Sí | N/D |
| Microsoft Hyper-V Server 2012 R2 | Sí | Sí | N/D |
| Windows Server 2012 | Sí | Sí | N/D |
| Windows Server 2008 R2 | Sí, una funcionalidad limitada | N/D | N/D |

> [!NOTE]
> Windows Admin Center requiere algunas características de PowerShell que no se incluyen en Windows Server 2008 R2, 2012 y 2012 R2. Si va a administrar estos elementos con Windows Admin Center, deberá instalar Windows Management Framework (WMF) versión 5.1 o superior en esos servidores.
> 
> Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior. 
> 
> Si no está instalado WMF, puede [descargar WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="deployment-options"></a>Opciones de implementación

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
| --------------------------------------------- | ------------------------------------------------- |----------------------------------------------|-------------------------------------------- |
|                                             |                                                 |                                              |                                            |

| Cliente local | Gateway Server | Servidor administrado | Clúster de conmutación por error |
| --- | --- | --- | --- |
| Instalar en un cliente local de Windows 10 que tenga conectividad con los servidores administrados.  Excelente para el inicio rápido, pruebas, ad hoc o escenarios de pequeña escala. |Instalar en un servidor de puerta de enlace designada y tener acceso desde cualquier explorador del cliente con conectividad al servidor de puerta de enlace.  Ideal para escenarios a gran escala. | Instalar directamente en un servidor administrado con el fin de administrar a sí mismo o a un clúster en los que es un nodo de miembro.  Ideal para escenarios distribuidos. | Implementar en un clúster de conmutación por error para habilitar la alta disponibilidad del servicio de puerta de enlace. Excelente para entornos de producción garantizar la resistencia de su servicio de administración. |

## <a name="high-availability"></a>Alta disponibilidad

Puede habilitar la alta disponibilidad del servicio de puerta de enlace mediante la implementación de Windows Admin Center en un modelo activo / pasivo en un clúster de conmutación por error. Si se produce un error en uno de los nodos del clúster, Windows Admin Center correctamente conmuta por error a otro nodo, lo que le permite seguir administrando los servidores en su entorno sin problemas.

[Aprenda a implementar Windows Admin Center con alta disponibilidad.](../deploy/high-availability.md)

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://aka.ms/windowsadmincenter)
