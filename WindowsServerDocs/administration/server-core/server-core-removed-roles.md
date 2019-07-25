---
title: 'Roles, servicios de función y características que no están en Windows Server: Server Core'
description: Obtenga información acerca de los roles y las características que no se incluyen en la opción de instalación Server Core para Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce5107e8e0ab573df7588428db65c8b223cf1f13
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476513"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Roles, servicios de función y características que no están en Windows Server: Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Los siguientes roles, servicios de rol y características se han quitado de la opción de instalación Server Core de Windows Server. Use esta información como ayuda para averiguar si la opción Server Core funciona para su entorno.

> [!NOTE]
> También puede ver una lista de los roles, servicios de función y características que [se incluyen en Server Core](server-core-roles-and-services.md). Es una lista muy grande, por lo que para obtener los mejores resultados, busque en esa lista la función o característica específica que le interese.

## <a name="roles-not-in-server-core"></a>Roles no en Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Servicios de rol que no están en Server Core
Tenga en cuenta que algunos servicios de rol de Escritorio remoto se incluyen en Server Core (agente de conexión, licencias, host de virtualización), pero otros no (puerta de enlace, host de sesión de escritorio remoto, acceso web).

- Print-Scan-Server
- Imprimir: Internet
- Puerta de enlace de RDS
- RDS-RD-Server
- RDS: acceso web
- Web-MGMT-Console
- Web-LGCY-MGMT-Console
- WDS: implementación
- WDS-transporte *(antes de la versión 1803 de Windows Server)*

## <a name="features-not-in-server-core"></a>Características no en Server Core

- BITS-IIS-ext
- BitLocker-NetworkUnlock
- Reproducción directa
- Internet-Print-Client
- LPR: monitor de Puerto
- MSMQ-multidifusión
- CMAK
- Asistencia remota
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-bits-servidor
- RSAT: NLB
- RSAT: SNMP
- RSAT-WINS
- Hyper-V-herramientas
- RSAT: herramientas de RDS
- RSAT: puerta de enlace de RDS
- RSAT-RDS-Licensing-diagnosis-UI
- RDS-licencias-interfaz de usuario
- UpdateServices-UI
- RSAT: ADCS
- RSAT-ADCS-MGMT
- RSAT: respondedor en línea
- RSAT: ADRMS
- RSAT: fax
- RSAT-File-Services
- RSAT-DFS-MGMT-con
- RSAT-FSRM-MGMT
- RSAT-NFS-admin
- RSAT: NPAS
- RSAT-Print-Services
- RSAT-VA-herramientas
- AdminPack de WDS
- SMTP-servidor
- TFTP-cliente
- WebDAV-redirector
- Biométrico: marco de trabajo
- Windows-Defender-GUI
- Windows-Identity-Foundation
- PowerShell-ISE
- Servicio de búsqueda
- Windows-TIFF-IFilter
- Redes inalámbricas
- Visor de XPS

