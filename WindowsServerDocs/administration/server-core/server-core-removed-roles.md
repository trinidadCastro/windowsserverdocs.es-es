---
title: 'Roles, servicios de función y características que no están en Windows Server: Server Core'
description: Obtenga información acerca de los roles y las características que no se incluyen en la opción de instalación Server Core para Windows Server.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce8fd0edc426b673f873717a27e6045e3476170f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383358"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Roles, servicios de función y características que no están en Windows Server: Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Los siguientes roles, servicios de rol y características se han quitado de la opción de instalación Server Core de Windows Server. Use esta información como ayuda para averiguar si la opción Server Core funciona para su entorno.

> [!NOTE]
> También puede ver una lista de los roles, servicios de función y características que [se incluyen en Server Core](server-core-roles-and-services.md). Es una lista muy grande, por lo que para obtener los mejores resultados, busque en esa lista la función o característica específica que le interese.

## <a name="roles-not-in-server-core"></a>Roles no en Server Core

- Servidor de fax (**fax**)
- Multipoint Services (**MultiPointServerRole**)
- Servicios de acceso y directivas de redes (**NPAS**)
- Servicios de implementación de Windows (**WDS**) *(antes de la versión 1803 de Windows Server)*

## <a name="role-services-not-in-server-core"></a>Servicios de rol que no están en Server Core
Tenga en cuenta que algunos servicios de rol de Escritorio remoto se incluyen en Server Core (agente de conexión, licencias, host de virtualización), pero otros no (puerta de enlace, host de sesión de escritorio remoto, acceso web).

- Servicios de impresión y documentos \ Servidor de digitalización distribuida (**Print-Scan-Server**)
- Servicios de impresión y documentos \ impresión en Internet (**Print-Internet**)
- Servicios de Escritorio remoto \ Escritorio remoto puerta de enlace (**RDS-Gateway**)
- Servicios de Escritorio remoto \ Escritorio remoto host de sesión (**RDS-Rd-Server**)
- Servicios de Escritorio remoto \ Escritorio remoto Web Access (**RDS-Web-Access**)
- Servicio de rol de servidor Web (IIS) \ herramientas de administración \ consola**de administración de IIS (Web-MGMT-Console**)
- Servicio de rol servidor Web (IIS) \ herramientas de administración \ compatibilidad con la administración de IIS 6 \ consola de administración de IIS 6 (**Web-LGCY-MGMT-Console**)
- Servicios de implementación de Windows \ servidor**de implementación (WDS-implementación**)
- Servicios de implementación de Windows \ servidor**de transporte (WDS-Transport**) *(antes de la versión 1803 de Windows Server)*

## <a name="features-not-in-server-core"></a>Características no en Server Core
- Servicio de transferencia inteligente en segundo plano (BITS) \ extensión del servidor IIS (**bits-IIS-ext**)
- Desbloqueo de red de BitLocker (**BitLocker-NetworkUnlock**)
- Direct Play (**Direct-Play**)
- Cliente de impresión en Internet (**Internet-Print-Client**)
- Monitor de puerto de LPR (**LPR: monitor de Puerto**)
- Message Queue Server \ compatibilidad con multidifusión de servicios de Message Queue Server (**MSMQ-multidifusión**)
- Kit de administración del administrador de conexiones RAS (**CMAK**)
- Asistencia remota (**asistencia remota**)
- Herramientas de administración remota del servidor \ herramientas de administración de características \ herramientas del servidor SMTP (**rsat-SMTP**)
- Herramientas de administración remota del servidor \ herramientas de administración de características \ Cifrado de unidad BitLocker utilidades de administración \ Cifrado de unidad BitLocker herramientas (**rsat-Feature-Tools-BitLocker-RemoteAdminTool**)
- Herramientas de administración remota del servidor \ herramientas de administración de características \ herramientas de extensiones de servidor BITS (**rsat-bits-servidor**)
- Herramientas de administración remota del servidor \ herramientas de administración de características \ herramientas de equilibrio**de carga de red (RSAT-NLB**)
- Herramientas de administración remota del servidor \ herramientas de administración de características \ herramientas**de SNMP (RSAT-SNMP**)
- Herramientas de administración remota del servidor \ herramientas de administración de características \ herramientas del servidor WINS (**rsat-WINS**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de administración de Hyper-V \ herramientas de administración de GUI de Hyper-V (**Hyper-v-Tools**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Servicios de Escritorio remoto Tools \ Escritorio remoto Gateway Tools (**rsat-RDS-Tools**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Servicios de Escritorio remoto Tools \ Escritorio remoto Gateway Tools (**rsat-RDS-Gateway**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Servicios de Escritorio remoto Tools \ Escritorio remoto herramientas de diagnóstico de licencias (**rsat-RDS-Licensing-Diagnostics-UI**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Servicios de Escritorio remoto Tools \ Escritorio remoto Licensing Tools (**RDS-Licensing-UI**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Windows Server Update Services Tools \ consola de administración**de interfaz de usuario (UpdateServices-UI**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Active Directory herramientas de servicios de certificados (**rsat-ADCS**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Active Directory herramientas de servicios de certificados \ herramientas**de administración de entidades de certificación (RSAT-ADCS-MGMT**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Active Directory herramientas de servicios de certificados \ herramientas de respuesta en línea (**rsat-online-respondedor**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Active Directory Rights Management Services Tools (**rsat-ADRMS**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas del servidor de fax (**rsat-fax**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de servicios de archivo (**rsat-File-Services**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de servicios de archivos \ herramientas de administración de DFS (**rsat-DFS-MGMT-con**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de servicios de archivo \ herramientas**de administrador de recursos de servidor de archivos (RSAT-FSRM-MGMT**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de servicios de archivo \ servicios para Network File System Management Tools (**rsat-NFS-admin**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de servicios de acceso y directivas de redes (**rsat-NPAS**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ Servicios de impresión y documentos herramientas (**rsat-Print-Services**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de activación por volumen (**rsat-va-Tools**)
- Herramientas de administración remota del servidor \ herramientas de administración de roles \ herramientas de servicios**de implementación de Windows (WDS-AdminPack**)
- Servicios simples de TCP/IP (**simple-TCPIP**)
- Servidor SMTP (**servidor SMTP**)
- Cliente TFTP (**TFTP-Client**)
- Redirector de WebDAV (**WebDAV-redirector**)
- Plataforma de biometría de Windows (**Biometric-Framework**)
- Características de Windows Defender \ GUI para Windows Defender (**Windows-Defender-GUI**)
- Windows Identity Foundation 3,5 (**Windows-Identity-Foundation**)
- Windows PowerShell \ Windows PowerShell ISE (**PowerShell-ISE**)
- Search Service de Windows (**servicio de búsqueda**)
- Windows TIFF IFilter (**Windows-TIFF-IFilter**)
- Servicio de LAN inalámbrica (**redes inalámbricas**)
- Visor de XPS (**visor de XPS**)
