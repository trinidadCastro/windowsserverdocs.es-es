---
title: Roles, servicios de rol y características no incluidas en Windows Server - Server Core
description: Obtenga información sobre los roles y características no incluidas en la opción de instalación Server Core de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825536"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Roles, servicios de rol y características no incluidas en Windows Server - Server Core

> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Se quitaron los siguientes roles, servicios de rol y características de la opción de instalación Server Core de Windows Server. Utilice esta información para ayudar a averiguar si la opción Server Core funciona para su entorno.

> [!NOTE]
> También puede ver una lista de los roles, servicios de rol y características que [se incluyen en Server Core](server-core-roles-and-services.md). Es una lista muy grande, de modo que para obtener mejores resultados, busque esa lista para el rol o característica específicos que le interesa.

## <a name="roles-not-in-server-core"></a>Funciones no en Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Servicios de rol no está en Server Core
Tenga en cuenta que algunos servicios de rol de escritorio remoto se incluyen en Server Core (agente de conexión, las licencias, Host de virtualización), pero otros no lo están (puerta de enlace, Host de sesión de RD Web Access).

- Servidor de digitalización de impresión
- Print-Internet
- RDS-Gateway
- Servidor de escritorio remoto de RDS
- RDS-Web-Access
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- Implementación de WDS
- Transporte de WDS *(antes de la versión 1803 de Windows Server)*

## <a name="features-not-in-server-core"></a>Características no incluidas en Server Core

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Direct-Play
- Internet-Print-Client
- LPR-Port-Monitor
- MSMQ-Multicasting
- CMAK
- Asistencia remota
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- Servidor de Bits de RSAT
- RSAT-NLB
- RSAT-SNMP
- RSAT WINS
- Hyper-V-Tools
- RSAT de RDS
- RSAT-RDS-Gateway
- RSAT RDS-licencias-diagnóstico de la interfaz de usuario
- UI licencias de RDS
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-Responder
- RSAT-ADRMS
- RSAT-Fax
- RSAT-File-Services
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- Administración de RSAT-NFS
- RSAT-NPAS
- RSAT-Print-Services
- Herramientas de evaluación de vulnerabilidad de RSAT
- WDS-AdminPack
- SMTP-Server
- Cliente TFTP
- WebDAV-Redirector
- Biometric-Framework
- Windows-Defender-Gui
- Windows-Identity-Foundation
- PowerShell-ISE
- Servicio de búsqueda
- Windows-TIFF-IFilter
- Acceso a la red inalámbrica
- XPS-Viewer

