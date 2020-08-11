---
title: Soluciones de administración relacionadas con Windows Admin Center
description: En este tema se explica cómo Windows Admin Center se compara con otras soluciones o productos de administración y supervisión de Microsoft (Proyecto Honolulu) y se complementa con ellos.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.openlocfilehash: 2f3a8de38cc643184468fccb4fcdd24f9ba75dd7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997522"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center y soluciones de administración relacionadas de Microsoft

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

[Windows Admin Center](../overview.md) es la evolución de las herramientas de administración tradicionales de servidores incluidas para situaciones en las que puede haber utilizado Escritorio remoto (RDP) para conectarse a un servidor para la solución de problemas o la configuración. No pretende sustituir a otras soluciones de administración de Microsoft existentes, sino que complementa a estas soluciones, tal y como se describe a continuación.

## <a name="remote-server-administration-tools-rsat"></a>Herramientas de administración remota del servidor (RSAT)

[Herramientas de administración remota del servidor (RSAT)](../../../remote/remote-server-administration-tools.md) es una colección de herramientas de GUI y de PowerShell para administrar roles y características opcionales en Windows Server. RSAT cuenta con muchas funcionalidades que no tiene Windows Admin Center. En el futuro, agregaremos algunas de las herramientas más usadas de RSAT para Windows Admin Center. Cualquier rol o característica nueva de Windows Server que requiera una interfaz gráfica de usuario para la administración estará en Windows Admin Center.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) es un servicio de administración de movilidad empresarial basado en la nube que te permite administrar dispositivos iOS, Android, Windows y macOS, basándose en un conjunto de directivas. Intune se centra en permitirte proteger la información de la empresa mediante el control de cómo tu personal accede y comparte la información. Por el contrario, Windows Admin Center no está basado en directivas, sino que permite la administración ad-hoc de los sistemas Windows 10 y Windows Server, mediante PowerShell y WMI remotos sobre WinRM.

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) es una plataforma híbrida en la nube que te permite entregar servicios de Azure desde tu centro de datos. Azure Stack se administra con PowerShell o el portal de administrador, que es similar a Azure Portal tradicional que se utiliza para acceder y administrar los servicios de Azure tradicionales. Windows Admin Center no está pensado para administrar la infraestructura de Azure Stack, pero puedes usarlo para [administrar las máquinas virtuales Azure IaaS](../azure/manage-azure-vms.md) (mediante la ejecución de Windows Server 2016, Windows Server 2012 R2, o Windows Server 2012) o solucionar problemas de servidores físicos individuales implementados en tu entorno de Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) es una solución de administración del centro de datos local para la implementación, configuración, administración y supervisión de todo el centro de datos. System Center te permite ver el estado de todos los sistemas de su entorno, mientras que Windows Admin Center te permite profundizar en un servidor específico para administrarlo o solucionar los problemas con herramientas más pormenorizadas.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Herramientas y plataforma "incluidas" rediseñadas** | **Administración y supervisión del centro de datos** |
| Incluidas con la licencia de Windows Server, **sin costo adicional**, al igual que MMC y otras herramientas tradicionales incluidas. | **Completo** conjunto de soluciones para obtener un valor agregado en todo tu entorno y plataformas. |
| Administración **ligera** y remota basada en explorador de las instancias de Windows Server, **en cualquier lugar**; alternativa a RDP | Administración y supervisión de sistemas **heterogéneos** **a escala**, incluidos Linux, VMware y Hyper-V |
|Exploración en **profundidad** con un único servidor único y clúster para solucionar problemas de configuración y mantenimiento|Aprovisionamiento de infraestructura; automatización y autoservicio; **variedad** de supervisión de infraestructuras y cargas de trabajo|
|Administración optimizada de clústeres **HCI** de 2 a 4 nodos **individuales**, integración de Hyper-V, espacios de almacenamiento directo y SDN|Implementación y administración de Hyper-V, clústeres de Windows Server a **escala del centro de datos** desde una **reconstrucción completa** con SCVMM|
|**Supervisión solo en HCI**; el servicio de mantenimiento de clústeres almacena el historial. Plataforma extensible para las **extensiones de herramientas de administración** propias y de terceros|Plataforma de **supervisión extensible** y **escalable** plataforma en SCOM, con alertas, notificaciones, supervisión de la carga de trabajo de otros fabricantes; SQL para historial|
|Puente más sencillo a **híbrido**; incorporación y uso de una variedad de servicios de Azure para protección de datos, replicación, actualizaciones y mucho más|Protección de datos, replicación, actualizaciones **integradas** (DPM/VMM/SCCM). Integración híbrida con Log Analytics y Service Map|
|**Principales características de la plataforma** de Windows Server: Servicio de migración de almacenamiento, réplica de almacenamiento, información del sistema, etc.|**Plataformas adicionales**: Automatización de Orchestrator y SMA. Integraciones con SCSM y otras herramientas de administración de servicios|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Cada una entrega un valor objetivo de forma independiente; **mejor juntas** con funcionalidades complementarias.