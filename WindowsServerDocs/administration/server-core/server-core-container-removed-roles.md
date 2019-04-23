---
title: 'Roles, servicios de rol y características no está en versión 1803 de contenedores: Windows Server, Server Core'
description: Obtenga información sobre los roles y características que se quitan de la imagen de contenedor de Server Core para Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873786"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Roles, servicios de rol y características no está en versión 1803 de contenedores: Windows Server, Server Core

> Se aplica a: Windows Server, versión 1803

En Windows Server, versión 1803, hemos [reduce el tamaño total de la imagen de contenedor de Server Core a **GB 1,58**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). La manera que hemos hecho esto es mediante la optimización de la arquitectura y eliminación de las cosas no es necesario en un [contenedor de Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Algunos eran las cosas que no funcionan en contenedores, algunas son roles y características que nadie ha estado usando. 

> [!IMPORTANT]
> Hemos quitado estos de Server Core **contenedor** la imagen, no [Server Core propio](server-core-roles-and-services.md). 

Esta es la lista completa de características y funciones que se quitan de la imagen de contenedor de Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Utilidades de BitLocker
<br>BitLocker
<br>BITS
<br>BITSExtensions-Upload
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS-Infrastructure
<br>Contenedores
<br>CoreFileServer
<br>DataCenterBridging-LLDP-Tools
<br>DataCenterBridging
<br>Núcleo de desduplicación
<br>DeviceHealthAttestationService
<br>Servidor de DFSN
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>DiskIo-QoS
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster-PowerShell
<br>Servicios de archivo
<br>FileServerVSSAgent
<br>Infraestructura de FRS
<br>Servicios de infraestructura de FSRM
<br>Infraestructura de FSRM
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-Package
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer-PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Concesión de licencias
<br>LightweightServer
<br>Microsoft-Hyper-V-Management-Clients
<br>Microsoft-Hyper-V-Offline
<br>Microsoft-Hyper-V-Online
<br>Microsoft-Hyper-V
<br>Microsoft-Windows-FCI-Client-Package
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-Subsystem-Linux
<br>MSRDC infraestructura
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Gui de cliente de impresión
<br>Printing-LPDPrintService
<br>Server-Foundation-funciones de impresión
<br>Printing-Server-Role
<br>QWAVE
<br>RasRoutingProtocols
<br>Servicios de escritorio remoto
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-Role
<br>RightsManagementServices
<br>RMS-Federation
<br>SBMgr-UI
<br>ServerCore-Drivers-General-WOW64
<br>ServerCore-Drivers-General
<br>ServerForNFS-Infrastructure
<br>ServerManager-Core-RSAT-Feature-Tools
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-Server
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>Storage-Replica-AdminPack
<br>Réplica de almacenamiento
<br>Cmdlets de PSH TPM
<br>UpdateServices-Database
<br>UpdateServices-Services
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Web-Application-Proxy
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-Server
<br>WSS-Product-Package

</div>