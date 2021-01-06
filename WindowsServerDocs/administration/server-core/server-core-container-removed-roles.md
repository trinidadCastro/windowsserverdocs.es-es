---
title: 'Roles, servicios de rol y características no en los contenedores Server Core: Windows Server, versión 1803'
description: Obtenga información sobre los roles y las características que se han quitado de la imagen de contenedor Server Core para Windows Server.
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.topic: conceptual
ms.openlocfilehash: 058a976a20e09cf46b57ec39af6c7f5b0cc30fdf
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947171"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Roles, servicios de rol y características no en los contenedores Server Core: Windows Server, versión 1803

> Se aplica a: Windows Server, versión 1803

En Windows Server, versión 1803, hemos [reducido el tamaño total de la imagen de contenedor Server Core a **1,58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). La manera de hacerlo es optimizar la arquitectura y quitar las cosas que no necesita en un [contenedor Server Core](/virtualization/windowscontainers/about/). Algunos eran cosas que no funcionaban en los contenedores, algunos eran roles y características que no se usaban.

> [!IMPORTANT]
> Se han quitado de la imagen de **contenedor** Server Core, no de [Server Core](server-core-roles-and-services.md).

Esta es la lista completa de las características y los roles que se han quitado de la imagen de contenedor Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Bitlocker-Utilities
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
<br>DataCenterBridging-LLDP-herramientas
<br>DataCenterBridging
<br>Dedup-Core
<br>DeviceHealthAttestationService
<br>DFSN-Server
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices: ADAM
<br>DirectoryServices-DomainController
<br>DiskIo-QoS
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster-PowerShell
<br>File-Services
<br>FileServerVSSAgent
<br>FRS-Infrastructure
<br>FSRM-Infrastructure-Services
<br>FSRM-Infrastructure
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-Package
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer: PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Licencias
<br>LightweightServer
<br>Microsoft-Hyper-V-Management-clients
<br>Microsoft-Hyper-V-sin conexión
<br>Microsoft-Hyper-V-en línea
<br>Microsoft-Hyper-V
<br>Paquete Microsoft-Windows-FCI-Client-
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-Subsystem-Linux
<br>MSRDC-Infrastructure
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Impresión: cliente-GUI
<br>Printing-LPDPrintService
<br>Impresión-Server-Foundation-Features
<br>Impresión: rol de servidor
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
<br>ServerCore-drivers-general-WOW64
<br>ServerCore-drivers-general
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
<br>Storage-réplica-AdminPack
<br>Storage-Replica
<br>TPM-PSH-cmdlets
<br>UpdateServices-Database
<br>UpdateServices-Services
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation: rol completo
<br>Proxy de aplicación Web
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-Server
<br>WSS-Product-Package

</div>