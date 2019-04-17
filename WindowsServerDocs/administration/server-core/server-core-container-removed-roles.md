---
title: Roles, servicios de rol y características no incluidas en versión de contenedores - Windows Server, Server Core 1803
description: Obtenga información sobre los roles y características que se quitan de la imagen de contenedor de Server Core de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1859924"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Roles, servicios de rol y características no incluidas en versión de contenedores - Windows Server, Server Core 1803

> Se aplica a: Windows Server, versión 1803

En Windows Server, versión 1803, hemos [reducido el tamaño total de la imagen de contenedor de Server Core a **1,58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). La forma en que hemos hecho esto es mediante la optimización de la arquitectura y eliminación de las cosas que no es necesario en un [contenedor de Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Algunas cosas que no ha funcionado en contenedores de eran, algunas eran roles y características que nadie ha estado utilizando. 

> [!IMPORTANT]
> Hemos eliminado estas desde la imagen de **contenedor** de Server Core, no [Server Core propio](server-core-roles-and-services.md). 

Aquí es la lista completa de las características y funciones que se quitan de la imagen de contenedor de Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Utilidades de BitLocker
<br>BitLocker
<br>BITS
<br>Carga de BITSExtensions
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>ServiciosCertificado
<br>Infraestructura de ClientForNFS
<br>Contenedores
<br>CoreFileServer
<br>DataCenterBridging-LLDP-Tools
<br>DataCenterBridging
<br>Dedup principales
<br>DeviceHealthAttestationService
<br>DFSN-Server
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices ADAM
<br>DirectoryServices-DomainController
<br>DiskIo QoS
<br>EnhancedStorage
<br>ClústerDeConmutadorAPruebaDeFallos AdminPak
<br>ClústerDeConmutadorAPruebaDeFallos AutomationServer
<br>ClústerDeConmutadorAPruebaDeFallos CmdInterface
<br>ClústerDeConmutadorAPruebaDeFallos FullServer
<br>ClústerDeConmutadorAPruebaDeFallos PowerShell
<br>Servicios de archivos
<br>FileServerVSSAgent
<br>Infraestructura de FRS
<br>Servicios de infraestructura FSRM
<br>FSRM infraestructura
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>Paquete de HostGuardianService
<br>IdentityServer SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Concesión de licencias
<br>LightweightServer
<br>Microsoft-Hyper-V--los clientes de administración
<br>Microsoft-Hyper-V-sin conexión
<br>Microsoft Hyper-V-en línea
<br>Microsoft-Hyper-V
<br>Microsoft Windows FCI cliente paquete
<br>Microsoft-Windows-directivas de grupo-ServerAdminTools-Update
<br>Microsoft-Windows-subsistema-Linux
<br>Infraestructura de MSRDC
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>PnrpOnly de P2P
<br>PeerDist
<br>Gui del cliente de impresión
<br>LPDPrintService de impresión
<br>Impresión-Server-Foundation-características
<br>Función de servidor de impresión
<br>QWAVE
<br>RasRoutingProtocols
<br>Servicios de escritorio remoto
<br>Acceso remoto
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-Role
<br>RightsManagementServices
<br>Federación de RMS
<br>Interfaz de usuario de SBMgr
<br>ServerCore-controladores-General-WOW64
<br>ServerCore-controladores-General
<br>Infraestructura de ServerForNFS
<br>ServerManager-principal-RSAT-Feature-Tools
<br>ServerMediaFoundation
<br>ServerMigration
<br>Sesión
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol Server
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>Almacenamiento de la réplica-AdminPack
<br>Réplica de almacenamiento
<br>Cmdlets de TPM Psh:
<br>Base de datos UpdateServices
<br>Servicios de UpdateServices
<br>UpdateServices WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-completa-Role
<br>Proxy de aplicación Web
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders Server
<br>Paquete del producto de WSS

</div>