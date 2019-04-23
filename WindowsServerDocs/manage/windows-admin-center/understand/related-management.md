---
title: Windows Admin Center relacionados con las soluciones de administración
description: Cómo Windows Admin Center compara con y complementa otros supervisión y administración de soluciones y productos de Microsoft (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 385a066cb828f58d698c2ca47e0553e996a77733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847326"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center y administración relacionadas con soluciones de Microsoft

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

[Windows Admin Center](windows-admin-center.md) es la evolución de servidor tradicional de cuadro de herramientas de administración para las situaciones donde es posible que ha usado escritorio remoto (RDP) para conectarse a un servidor para solucionar problemas o configuración. No se ha diseñado para reemplazar otras soluciones de administración de Microsoft existentes; en su lugar complementa estas soluciones, tal como se describe a continuación.

## <a name="remote-server-administration-tools-rsat"></a>Herramientas de administración remota del servidor (RSAT)

[Herramientas de administración remota de servidor (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) es una colección de herramientas de GUI y de PowerShell para administrar opcionales roles y características en Windows Server. RSAT tiene muchas funcionalidades que no tiene Windows Admin Center. Podemos agregamos algunas de las herramientas más usadas de RSAT para Windows Admin Center en el futuro. En Windows Admin Center será un nuevo rol de servidor de Windows o la característica que requiere una interfaz gráfica de usuario para la administración.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) es un servicio de administración de movilidad empresarial basada en la nube que le permite administrar dispositivos iOS, Android, Windows y macOS, en función de un conjunto de directivas. Intune se centra en lo que le permite proteger la información de empresa mediante el control cómo su personal tiene acceso y comparte información. En cambio, Windows Admin Center no está controlado por directivas, pero permite la administración de ad hoc de los sistemas Windows 10 y Windows Server, mediante PowerShell y la WMI remota a través de WinRM.

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) es una plataforma de nube híbrida que le permite proporcionar servicios de Azure desde su centro de datos. Azure Stack se administra con PowerShell o el portal de administrador, que es similar a Azure portal tradicional usa para acceder y administrar servicios de Azure tradicionales. Windows Admin Center no está diseñada para administrar la infraestructura de Azure Stack, pero puede utilizarlo para [administrar máquinas virtuales de IaaS de Azure](../configure/manage-azure-vms.md) (que se ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012) o solucionar problemas servidores físicos individuales implementados en su entorno de Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) es una solución de administración de centro de datos local para la implementación, configuración, administración, supervisión de su centro de datos completo. System Center le permite ver el estado de todos los sistemas en su entorno, mientras que Windows Admin Center le permite profundizar en un servidor específico para administrar o solucionar problemas con las herramientas más granulares.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Rediseñado de herramientas y plataforma "en el cuadro"** | **Administración de centro de datos y supervisión** |
| Incluido con licencia de Windows Server – **sin costo adicional**, al igual que MMC y otras herramientas en el cuadro tradicionales | **Completa** conjunto de soluciones de valor añadido en su entorno y plataformas |
| **Ligero**, administración remota basada en Explorador de instancias de Windows Server, **en cualquier lugar**; alternativa a RDP | Administración y supervisión **heterogéneos** sistemas **a escala**, incluidas Linux, VMware y Hyper-V |
|**Deep** servidor único & único clúster exploración en profundidad para solucionar problemas de configuración y mantenimiento|Infraestructura de aprovisionamiento; automatización y autoservicio;  infraestructura y la carga de trabajo supervisión **amplitud**|
|Optimizado para la administración de **individuales** nodo 2 a 4 **HCI** clústeres, la integración de Hyper-V, espacios de almacenamiento directo y SDN|Implementar y administrar Hyper-V, clústeres de Windows Server en **escala de centro de datos** desde **bare metal** con SCVMM|
|**Supervisión en HCI** solo; servicio de mantenimiento de clúster almacena el historial. Una plataforma extensible para la entidad de 1 y 3 **extensiones de herramientas de administración**|**Extensible** & **supervisión escalable** plataforma en SCOM, con alertas, notificaciones, carga de trabajo de otros fabricantes supervisión; SQL para el historial|
|Puente más fácil a **híbrida**; incorporar y usar una variedad de servicios de Azure para la protección de datos, replicación, actualizaciones y mucho más|**Integrada** protección de datos, replicación, las actualizaciones (DPM/VMM/SCCM). Integración híbrida con Log Analytics y Service Map|
|**Características de la plataforma se ilumina** de Windows Server: Información de sistema Migration Service, réplica de almacenamiento, almacenamiento, etcetera.|**Plataformas adicionales**: Automatización de Orchestrator y SMA. Integraciones con otras herramientas de administración de servicio & de SCSM|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Cada una ofrece de forma independiente; el valor de destino **mejor juntos** con funciones complementarias.
