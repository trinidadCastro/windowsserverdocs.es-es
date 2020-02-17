---
title: ¿Qué tipo de instalación es adecuada para ti?
description: En este tema se describen las diferentes opciones de instalación para Windows Admin Center, incluida la instalación en un equipo con Windows 10 o en Windows Server para que lo usen varios administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: 503cd64cac0673829fe21bc15e8ad9d6a83bbb15
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950516"
---
# <a name="what-type-of-installation-is-right-for-you"></a>¿Qué tipo de instalación es la adecuada para ti?

En este tema se describen las diferentes opciones de instalación para Windows Admin Center, incluida la instalación en un equipo con Windows 10 o en Windows Server para que lo usen varios administradores. Para instalar Windows Admin Center en una VM en Azure, consulta [Implementación de Windows Admin Center en Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Instalación: Tipos

![img](../media/deployment-options/install-options.PNG)

| Cliente local                                | Servidor de puerta de enlace                                  | Servidor administrado                               | Clúster de conmutación por error                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [Instalación](../deploy/install.md) en un cliente de Windows 10 local que tiene conectividad con los servidores administrados.  Excelente para escenarios de inicio rápido, pruebas, ad hoc o de pequeña escala. |[Instalación](../deploy/install.md) en un servidor de puerta de enlace designado y con acceso desde cualquier explorador del cliente con conectividad con el servidor de puerta de enlace.  Ideal para escenarios a gran escala. | [Instalación](../deploy/install.md) directa en un servidor administrado para administrarse a sí mismo o en un clúster en el que es un nodo miembro.  Excelente para escenarios distribuidos. | [Implementación](#high-availability) en un clúster de conmutación por error para permitir la alta disponibilidad del servicio de puerta de enlace. Genial para entornos de producción para garantizar la resistencia del servicio de administración. |

## <a name="installation-supported-operating-systems"></a>Instalación: Sistemas operativos compatibles

Puedes **instalar** Windows Admin Center en los siguientes sistemas operativos de Windows:

| **Plataforma**                       | **Modo de instalación** |
| -----------------------------------| --------------------- |
| Windows 10                         | Cliente local |
| Canal semianual de Windows Server | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |
| Windows Server 2016                | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |
| Windows Server 2019                | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |

Para utilizar Windows Admin Center:

- **En un escenario de cliente local:** inicia la puerta de enlace de Windows Admin Center desde el menú Inicio y conéctate desde un explorador web del cliente mediante el acceso a `https://localhost:6516`.
- **En otros escenarios:** conéctate a la puerta de enlace de Windows Admin Center desde un explorador del cliente en un equipo diferente, por ejemplo, `https://servername.contoso.com`

> [!WARNING]
> No se admite la instalación de Windows Admin Center en un controlador de dominio. [Obtén más información sobre los procedimientos recomendados de seguridad del controlador de dominio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack).

## <a name="installation-supported-web-browsers"></a>Instalación: Exploradores web compatibles

Microsoft Edge (incluido [Microsoft Edge Insider](https://microsoftedgeinsider.com)) y Google Chrome se han probado y admitido en Windows 10. Otros exploradores web, incluido Internet Explorer y Firefox, no forman parte actualmente de nuestra matriz de pruebas y, por lo tanto, no se admiten *oficialmente*. Es posible que estos exploradores tengan problemas al ejecutar Windows Admin Center. Por ejemplo, Firefox tiene su propio almacén de certificados, por lo que debes importar el certificado `Windows Admin Center Client` a Firefox para usar Windows Admin Center en Windows 10. Para obtener más información, consulta los [problemas conocidos específicos del explorador](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Destino de administración: Sistemas operativos compatibles

Puedes **administrar** los siguientes sistemas operativos de Windows mediante Windows Admin Center:

| Version | Administrar *nodo* mediante *Administrador del servidor* | Administrar mediante *Administrador de clústeres* |
| ------------------------- |--------------- | ----- |
| Windows 10 | Sí (mediante Administración de equipos) | N/A |
| Canal semianual de Windows Server | Sí | Sí |
| Windows Server 2019 | Sí | Sí |
| Windows Server 2016 | Sí | Sí, con la [última actualización acumulativa](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sí | Sí |
| Windows Server 2012 R2 | Sí | Sí |
| Microsoft Hyper-V Server 2012 R2 | Sí | Sí |
| Windows Server 2012 | Sí | Sí |
| Windows Server 2008 R2 | Sí, funcionalidad limitada | N/A |

> [!NOTE]
> Windows Admin Center requiere características de PowerShell que no se incluyen en Windows Server 2008 R2, 2012 y 2012 R2. Si los vas a administrar con Windows Admin Center, deberás instalar Windows Management Framework (WMF), versión 5.1 o posterior, en esos servidores.
> 
> Escribe `$PSVersiontable` en PowerShell para verificar que esté instalado WMF y que la versión sea 5.1 o posterior. 
> 
> Si WMF no está instalado, puedes [descargar WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="high-availability"></a>Alta disponibilidad

Puedes habilitar la alta disponibilidad del servicio de puerta de enlace mediante la implementación de Windows Admin Center en un modelo activo-pasivo en un clúster de conmutación por error. Si se produce un error en uno de los nodos del clúster, Windows Admin Center conmuta por error correctamente a otro nodo, lo que te permite seguir administrando los servidores de tu entorno sin problemas.

[Más información sobre la implementación de Windows Admin Center con alta disponibilidad.](../deploy/high-availability.md)

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://aka.ms/windowsadmincenter)
