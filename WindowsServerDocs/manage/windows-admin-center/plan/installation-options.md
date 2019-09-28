---
title: ¿Qué tipo de instalación es adecuada para usted?
description: En este tema se describen las distintas opciones de instalación del centro de administración de Windows, incluida la instalación de en un equipo con Windows 10 o Windows Server para su uso por parte de varios administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 144c57bba621ee1b94a66914f8d9b6c0292f8b03
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406871"
---
# <a name="what-type-of-installation-is-right-for-you"></a>¿Qué tipo de instalación es el adecuado para ti?

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En este tema se describen las distintas opciones de instalación del centro de administración de Windows, incluida la instalación de en un equipo con Windows 10 o Windows Server para su uso por parte de varios administradores. Para instalar el centro de administración de Windows en una máquina virtual de Azure, consulte [implementación del centro de administración de Windows en Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Instalación: Tipos

| Cliente local                                | Servidor de puerta de enlace                                  | Servidor administrado                               | Clúster de conmutación por error                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| ![IMG](../media/deployment-options/W10.PNG) | ![IMG](../media/deployment-options/gateway.PNG) | ![IMG](../media/deployment-options/node.PNG) | ![IMG](../media/deployment-options/HA.png) |
| Instale en un cliente de Windows 10 local que tenga conectividad a los servidores administrados.  Excelente para escenarios de inicio rápido, pruebas, ad hoc o de escala pequeña. |Instale en un servidor de puerta de enlace designado y acceda desde cualquier explorador cliente con conectividad al servidor de puerta de enlace.  Excelente para escenarios a gran escala. | Instale directamente en un servidor administrado con el fin de administrarse a sí mismo o de un clúster en el que sea un nodo de miembro.  Excelente para escenarios distribuidos. | Implemente en un clúster de conmutación por error para habilitar la alta disponibilidad del servicio de puerta de enlace. Ideal para entornos de producción con el fin de garantizar la resistencia del servicio de administración. |

## <a name="installation-supported-operating-systems"></a>Instalación: Sistemas operativos compatibles

Puede **instalar** el centro de administración de Windows en los siguientes sistemas operativos de Windows:

| **Plataforma**                       | **Modo de instalación** |
| -----------------------------------| --------------------- |
| Windows 10, versión 1709 o posterior  | Cliente local |
| Canal semianual de Windows Server | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |
| Windows Server 2016                | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |
| Windows Server 2019                | Servidor de puerta de enlace, servidor administrado, clúster de conmutación por error |

Al centro de administración de Windows operativo:

- **En escenario de cliente local:** Inicie la puerta de enlace del centro de administración de Windows desde el menú Inicio y conéctese a él desde un explorador Web del cliente mediante el acceso a `https://localhost:6516`.
- **En otros escenarios:** Conéctese a la puerta de enlace del centro de administración de Windows en otra máquina desde un explorador cliente a través de su dirección URL, por ejemplo, `https://servername.contoso.com`

> [!WARNING]
> No se admite la instalación del centro de administración de Windows en un controlador de dominio. [Obtenga más información sobre los procedimientos recomendados de seguridad de los controladores de dominio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

## <a name="installation-supported-web-browsers"></a>Instalación: Exploradores Web compatibles

Microsoft Edge y Google Chrome se prueban y admiten en Windows 10. Otros exploradores Web, incluidos Internet Explorer y Firefox, no forman parte actualmente de nuestra matriz de pruebas y, por lo tanto, no se admiten *oficialmente* . Es posible que estos exploradores tengan problemas al ejecutar el centro de administración de Windows. Por ejemplo, Firefox tiene su propio almacén de certificados, por lo que debe importar el certificado `Windows Admin Center Client` en Firefox para usar el centro de administración de Windows en Windows 10. Para obtener más información, consulte [problemas conocidos específicos del explorador](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Destino de administración: Sistemas operativos compatibles

Puede **administrar** los siguientes sistemas operativos de Windows mediante el centro de administración de Windows:

| `Version` | Administrar *nodo* a través de *Administrador del servidor* | Administrar el *clúster* a través de *Administrador de clústeres de conmutación por error* | Administrar *HCI* a través *del administrador de clústeres de HCI* |
| ------------------------- |--------------- | ----- | ------------------------ |
| Windows 10, versión 1709 o posterior | Sí (a través de administración de equipos) | N/D | N/D |
| Canal semianual de Windows Server | Sí | Sí | N/D |
| Windows Server 2019 | Sí | Sí | Sí |
| Windows Server 2016 | Sí | Sí | Sí, con la [última actualización acumulativa](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sí | Sí | N/D |
| Windows Server 2012 R2 | Sí | Sí | N/D |
| Microsoft Hyper-V Server 2012 R2 | Sí | Sí | N/D |
| Windows Server 2012 | Sí | Sí | N/D |
| Windows Server 2008 R2 | Sí, funcionalidad limitada | N/D | N/D |

> [!NOTE]
> El centro de administración de Windows requiere características de PowerShell que no están incluidas en Windows Server 2008 R2, 2012 y 2012 R2. Si va a administrarlos con el centro de administración de Windows, tendrá que instalar Windows Management Framework (WMF) versión 5,1 o superior en esos servidores.
> 
> Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior. 
> 
> Si WMF no está instalado, puede [Descargar wmf 5,1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="high-availability"></a>Alta disponibilidad

Puede habilitar la alta disponibilidad del servicio de puerta de enlace mediante la implementación del centro de administración de Windows en un modelo activo-pasivo en un clúster de conmutación por error. Si se produce un error en uno de los nodos del clúster, el centro de administración de Windows conmuta por error correctamente a otro nodo, lo que le permite seguir administrando los servidores de su entorno sin problemas.

[Obtenga información acerca de cómo implementar el centro de administración de Windows con alta disponibilidad.](../deploy/high-availability.md)

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://aka.ms/windowsadmincenter)
