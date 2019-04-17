---
title: Windows Admin Center relacionados con las soluciones de administración
description: Cómo Windows Admin Center se compara con y complementa otras supervisión y administración de soluciones y productos de Microsoft (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296717"
---
# Windows Admin Center y las soluciones de administración relacionados de Microsoft

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

[Windows Admin Center](windows-admin-center.md) es la evolución de herramientas de administración de servidor en el cuadro tradicionales para situaciones donde es posible que hayas usado escritorio remoto (RDP) para conectarse a un servidor para solucionar problemas o configuración. No está pensado para reemplazar otras soluciones de administración de Microsoft existentes; en su lugar, complementa estas soluciones, tal como se describe a continuación.

## Herramientas de administración remota del servidor (RSAT)

[Herramientas de administración remota de servidor (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) es una colección de herramientas de interfaz gráfica de usuario y PowerShell para administrar opcionales roles y características en Windows Server. RSAT tiene muchas funcionalidades que no tiene Windows Admin Center. Podemos agregar algunas de las herramientas más utilizadas en RSAT en Windows Admin Center en el futuro. Cualquier nuevo rol de servidor de Windows o una característica que requiere una interfaz gráfica de usuario para la administración estará en Windows Admin Center.

## Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) es un servicio de administración de movilidad empresarial basada en la nube que te permite administrar dispositivos iOS, Android, Windows y Mac OS, en función de un conjunto de directivas. Intune se centra en lo que te permite proteger la información de empresa mediante el control de cómo sus empleados accede y comparten la información. En cambio, Windows Admin Center no está controlada por directivas, pero permite la administración de ad-hoc de sistemas Windows 10 y Windows Server, usando PowerShell y WMI remotas a través de WinRM.

## Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) es una plataforma de nube híbrida que te permite ofrecer servicios de Azure desde el centro de datos. Pila de Azure se administra con PowerShell o el portal de administrador, que es similar al Azure portal tradicional que se usa para acceder a y administrar los servicios de Azure tradicionales. Windows Admin Center no está pensado para administrar la infraestructura de la pila de Azure, pero puedes usarla para [administrar máquinas virtuales de IaaS de Azure](../azure/manage-azure-vms.md) (ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012) o solucionar problemas de física individual servidores implementados en tu entorno Azure Stack.

## System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) es una solución de administración de centro de datos local para la implementación, configuración, administración, supervisión de su centro de datos completo. System Center te permite ver el estado de todos los sistemas en su entorno, mientras que Windows Admin Center te permite profundizar en un servidor específico para administrar o solucionar el problema con las herramientas más detalladas.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Plataforma de "en el equipo" rediseñado & herramientas** | **Supervisión de & de administración de centros de datos** |
| Incluido con licencia de Windows Server: **ningún coste adicional**, al igual que MMC y otras herramientas integradas tradicionales | Conjunto **completo** de soluciones para un valor adicional en tu entorno y plataformas |
| **Ligero**, basada en explorador administración remota de instancias de Windows Server, **en cualquier lugar**; alternativa a RDP | Administrar & monitor **heterogéneos** sistemas **a escala**, incluyendo Hyper-V, VMware y Linux |
|**Deep** & de servidor único único clúster desglose para solución de problemas, mantenimiento de & la configuración|Infraestructura de aprovisionamiento; autoservicio; y automatización  infraestructura y carga de trabajo de supervisión **amplitud**|
|Administración optimizada **individuales** de 2 a 4 **HCI** de clústeres de nodos, integración de Hyper-V, espacios de almacenamiento directo y SDN|Implementar & administrar Hyper-V, clústeres de Windows Server a **escala de centros de datos** de **reconstrucción completa** con SCVMM|
|**Supervisión de HCI** solo; servicio de mantenimiento de clúster almacena el historial. Plataforma extensible para terceros 1 y 3 de **extensiones de herramienta de administración**|**Extensible** & plataforma de**supervisión escalable** en SCOM, con la alerta, notificaciones, carga de trabajo de terceros supervisión; SQL para historial|
|Puente de dispositivo más sencilla para **híbrido**; incorporar y usar una variedad de servicios de Azure para protección de datos, replicación, actualizaciones y mucho más|Protección de datos **integrada** , la replicación, actualizaciones (DPM/VMM/SCCM). Integración de híbrida con Log Analytics y servicio de mapa|
|**Mejore las características de plataforma** de Windows Server: servicio de migración de almacenamiento, réplica de almacenamiento, información del sistema, etcetera.|**Plataformas adicionales**: automatización en Orchestrator/SMA. Integración con SCSM & otras herramientas de administración de servicio|

#### Cada uno de ellos ofrece de forma independiente; el valor de destino **mejor juntos** con capacidades complementarias.
