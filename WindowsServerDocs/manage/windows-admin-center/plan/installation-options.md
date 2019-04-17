---
title: ¿Qué tipo de instalación es adecuado para TI
description: En este tema se describe las opciones de instalación diferente para Windows Admin Center, incluida la instalación en un equipo Windows 10 o un servidor de Windows para su uso por varios de los administradores.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: cc6f9e6c2f2572618c1c5fdae00047d24fbeb3cf
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296667"
---
# ¿Qué tipo de instalación es el adecuado para ti?

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

En este tema se describe las opciones de instalación diferente para Windows Admin Center, incluida la instalación en un equipo Windows 10 o un servidor de Windows para su uso por varios de los administradores. Para instalar Windows Admin Center en una máquina virtual en Azure, consulta [Implementar Windows Admin Center en Azure](../azure/deploy-wac-in-azure.md).

## Sistemas operativos compatibles: instalación

Puedes **instalar** Windows Admin Center en los siguientes sistemas operativos de Windows:

| **Versión** | **Modo de instalación** |
|-------------|-----------------------|
|Windows 10, versión 1709 o posterior | Modo de escritorio |
|Canal semianual de WindowsServer | Modo de puerta de enlace |
|WindowsServer2016 | Modo de puerta de enlace |
|Windows Server 2019 | Modo de puerta de enlace |

**Modo de escritorio:** Iniciar desde el menú Inicio y conectarse a la puerta de enlace de Windows Admin Center desde el mismo equipo en el que está instalado (es decir, `https://localhost:6516`)

**El modo de puerta de enlace:** Conectarse a la puerta de enlace de Windows Admin Center desde un explorador cliente en un equipo diferente (es decir, `https://servername.contoso.com`) 

> [!WARNING]
> No se admite la instalación de Windows Admin Center en un controlador de dominio. [Lee más sobre los procedimientos recomendados de seguridad de controlador de dominio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> No puedes usar Internet Explorer para administrar Windows Admin Center, en su lugar, debes usar un [admite el explorador](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Si vas a instalar Windows Admin Center en Windows Server, se recomienda administrar al conectarse de forma remota con Windows 10 y Edge.  Como alternativa, puede administrar localmente en Windows Server si has instalado un explorador compatible.

## Sistemas operativos compatibles: administración

Puedes **Administrar** los siguientes sistemas operativos de Windows con Windows Admin Center:

| Versión | Administrar el *nodo* a través del *Administrador del servidor* | Administrar el *clúster* a través del *Administrador de clústeres de conmutación por error* | Administrar *HCI* a través del *Administrador de clústeres de HCI*|
|-------------------------|---------------|-----|------------------------|
| Windows 10, versión 1709 o posterior | Sí (a través de administración de equipos) | N/D | N/D |
| Canal semianual de WindowsServer | Sí | Sí | N/D |
| Windows Server 2019 | Sí | Sí | Sí |
| WindowsServer2016 | Sí | Sí | Sí, con la [actualización acumulativa más reciente](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Sí | Sí | N/D |
| WindowsServer2012R2 | Sí | Sí | N/D |
| Microsoft Hyper-V Server 2012 R2 | Sí | Sí | N/D |
| Windows Server 2012 | Sí | Sí | N/D |
| Windows Server 2008 R2 | Sí, una funcionalidad limitada | N/D | N/D |

> [!NOTE]
> Windows Admin Center requiere características de PowerShell que no se incluyen en Windows Server 2008 R2, 2012 y 2012 R2. Si vas a administrar estos con Windows Admin Center, debes instalar Windows Management Framework (WMF) versión 5.1 o posterior en esos servidores.

>Escribe `$PSVersiontable` en PowerShell para comprobar que esté instalado WMF y que la versión sea 5.1 o posterior. 

>Si no está instalado WMF, puedes [Descargar WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## Opciones de implementación

| ![IMG](../media/deployment-options/W10.png) | ![IMG](../media/deployment-options/gateway.png) | ![IMG](../media/deployment-options/node.png) | ![IMG](../media/deployment-options/HA.png) |
|---|---|---|---|

| Cliente local | Servidor de puerta de enlace | Servidor administrado | Clúster de conmutación por error |
| --- | --- | --- | --- |
| Instalar en un cliente local de Windows 10 que tiene conectividad a los servidores administrados.  Ideal para inicio rápido, pruebas, ad-hoc o escenarios de pequeña escala. |Instalar en un servidor de puerta de enlace designado y el acceso desde cualquier explorador cliente con la conectividad con el servidor de puerta de enlace.  Ideal para escenarios a gran escala. | Instalar directamente en un servidor administrado con el fin de administrar propio o en un clúster en el que es un nodo miembro.  Ideal para escenarios distribuidos. | Implementar en un clúster de conmutación por error para habilitar la alta disponibilidad del servicio de puerta de enlace. Ideal para entornos de producción garantizar la resistencia de su servicio de administración. |

## Alta disponibilidad

Puedes habilitar la alta disponibilidad del servicio de puerta de enlace mediante la implementación de Windows Admin Center en un modelo de activo / pasivo en un clúster de conmutación por error. Si se produce un error en uno de los nodos del clúster, Windows Admin Center correctamente conmutar por error a otro nodo, lo que te permite continuar con la administración de servidores en su entorno sin problemas.

[Aprende a implementar Windows Admin Center con alta disponibilidad.](../deploy/high-availability.md)

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://aka.ms/windowsadmincenter)
