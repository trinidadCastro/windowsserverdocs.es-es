---
title: ¿Qué tipo de instalación es adecuada para usted?
description: En este tema se describen las distintas opciones de instalación del centro de administración de Windows, incluida la instalación de en un equipo con Windows 10 o Windows Server para su uso por parte de varios administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: 503cd64cac0673829fe21bc15e8ad9d6a83bbb15
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950516"
---
# <a name="what-type-of-installation-is-right-for-you"></a>¿Qué tipo de instalación es el adecuado para ti?

En este tema se describen las distintas opciones de instalación del centro de administración de Windows, incluida la instalación de en un equipo con Windows 10 o Windows Server para su uso por parte de varios administradores. Para instalar el centro de administración de Windows en una máquina virtual de Azure, consulte [implementación del centro de administración de Windows en Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Instalación: tipos

![img](../media/deployment-options/install-options.PNG)

| Cliente local                                | Servidor de puerta de enlace                                  | Servidor administrado                               | Clúster de conmutación por error                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [Instale](../deploy/install.md) en un cliente de Windows 10 local que tenga conectividad a los servidores administrados.  Excelente para escenarios de inicio rápido, pruebas, ad hoc o de escala pequeña. |[Instale](../deploy/install.md) en un servidor de puerta de enlace designado y acceda desde cualquier explorador cliente con conectividad al servidor de puerta de enlace.  Excelente para escenarios a gran escala. | [Instale](../deploy/install.md) directamente en un servidor administrado con el fin de administrarse a sí mismo o de un clúster en el que sea un nodo de miembro.  Excelente para escenarios distribuidos. | [Implemente](#high-availability) en un clúster de conmutación por error para habilitar la alta disponibilidad del servicio de puerta de enlace. Ideal para entornos de producción con el fin de garantizar la resistencia del servicio de administración. |

## <a name="installation-supported-operating-systems"></a>Instalación: sistemas operativos compatibles

Puede **instalar** el centro de administración de Windows en los siguientes sistemas operativos de Windows:

| **Plataforma**                       | **Modo de instalación** |
| -----------------------------------| --------------------- |
| 10 de Windows                         | Cliente local |
| Canal semianual de Windows Server | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |
| Windows Server 2016                | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |
| Windows Server 2019                | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |

Al centro de administración de Windows operativo:

- **En escenario de cliente local:** Inicie la puerta de enlace del centro de administración de Windows desde el menú Inicio y conéctese a él desde un explorador Web del cliente mediante el acceso a `https://localhost:6516`.
- **En otros escenarios:** Conéctese a la puerta de enlace del centro de administración de Windows en otra máquina desde un explorador cliente a través de su dirección URL, por ejemplo, `https://servername.contoso.com`

> [!WARNING]
> No se admite la instalación del centro de administración de Windows en un controlador de dominio. [Obtenga más información sobre los procedimientos recomendados de seguridad de los controladores de dominio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack).

## <a name="installation-supported-web-browsers"></a>Instalación: exploradores Web compatibles

Microsoft Edge (incluido [Microsoft Edge Insider](https://microsoftedgeinsider.com)) y Google Chrome se prueban y admiten en Windows 10. Otros exploradores Web, incluidos Internet Explorer y Firefox, no forman parte actualmente de nuestra matriz de pruebas y, por lo tanto, no se admiten *oficialmente* . Es posible que estos exploradores tengan problemas al ejecutar el centro de administración de Windows. Por ejemplo, Firefox tiene su propio almacén de certificados, por lo que debe importar el certificado de `Windows Admin Center Client` en Firefox para usar el centro de administración de Windows en Windows 10. Para obtener más información, consulte [problemas conocidos específicos del explorador](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Destino de administración: sistemas operativos compatibles

Puede **administrar** los siguientes sistemas operativos de Windows mediante el centro de administración de Windows:

| Version | Administrar *nodo* a través de *Administrador del servidor* | Administrar mediante el *Administrador de clústeres* |
| ------------------------- |--------------- | ----- |
| 10 de Windows | Sí (a través de administración de equipos) | N/A |
| Canal semianual de Windows Server | Sí | Sí |
| Windows Server 2019 | Sí | Sí |
| Windows Server 2016 | Sí | Sí, con la [última actualización acumulativa](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sí | Sí |
| R2 de Windows 2012 Server | Sí | Sí |
| Microsoft Hyper-V Server 2012 R2 | Sí | Sí |
| Windows Server 2012 | Sí | Sí |
| Windows Server 2008 R2 | Sí, funcionalidad limitada | N/A |

> [!NOTE]
> El centro de administración de Windows requiere características de PowerShell que no están incluidas en Windows Server 2008 R2, 2012 y 2012 R2. Si va a administrarlos con el centro de administración de Windows, tendrá que instalar Windows Management Framework (WMF) versión 5,1 o superior en esos servidores.
> 
> Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior. 
> 
> Si WMF no está instalado, puede [Descargar wmf 5,1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="high-availability"></a>Alta disponibilidad

Puede habilitar la alta disponibilidad del servicio de puerta de enlace mediante la implementación del centro de administración de Windows en un modelo activo-pasivo en un clúster de conmutación por error. Si se produce un error en uno de los nodos del clúster, el centro de administración de Windows conmuta por error correctamente a otro nodo, lo que le permite seguir administrando los servidores de su entorno sin problemas.

[Obtenga información acerca de cómo implementar el centro de administración de Windows con alta disponibilidad.](../deploy/high-availability.md)

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://aka.ms/windowsadmincenter)
