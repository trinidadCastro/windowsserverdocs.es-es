---
title: Roles, servicios de rol y características incluidos en Windows Server-Server Core
description: ¿Qué roles y características se incluyen en la opción de instalación Server Core de Windows Server?
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 1569feb27a75815771cf84317bebb2fde9d83dfa
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895866"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Roles, servicios de rol y características incluidos en Windows Server-Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Por lo general, hablamos de [lo que *no* está en Server Core](server-core-removed-roles.md) ; ahora vamos a probar un enfoque diferente y le indicamos lo que está *incluido* y si hay algo *instalado de forma predeterminada*. Los siguientes roles, servicios de rol y características se encuentran *en* la opción de instalación Server Core de Windows Server. Use esta información como ayuda para averiguar si la opción Server Core funciona para su entorno. Dado que se trata de una lista grande, considere la posibilidad de buscar el rol o la característica específicos que le interesen: si esa búsqueda no devuelve lo que está buscando, no se incluye en Server Core.

Por ejemplo, si busca "host de sesión de Escritorio remoto", no lo encontrará en esta página. Esto se debe a que el host de sesión de escritorio remoto no está incluido en la imagen de Server Core.

Recuerde que siempre puede [Ver](server-core-removed-roles.md) lo que *no* se incluye. Se trata simplemente de una manera diferente de ver las cosas.

## <a name="roles-included-in-server-core"></a>Roles incluidos en Server Core
La opción de instalación Server Core incluye los siguientes roles de servidor.

| Role                                            | Nombre                           | ¿Está instalado de forma predeterminada? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servicios de certificados de Active Directory           | AD-Certificate                 | N                     |
| Active Directory Domain Services                | AD-Domain-Services             | N                     |
| Servicios de federación de Active Directory            | ADFS-Federation                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Atestación de estado de dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Servicios de archivos y almacenamiento                       | FileAndStorage-servicios        | Y                     |
| Servicio de protección de host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Servicios de impresión y documentos                     | Print-Services                 | N                     |
| Acceso remoto                                   | RemoteAccess                   | N                     |
| Servicios de Escritorio remoto                         | Servicios de escritorio remoto        | N                     |
| Volume Activation Services                      | VolumeActivation               | N                     |
| Servidor web IIS                                  | Web-Server                     | N                     |
| Experiencia con Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Servicios de rol incluidos en Server Core
La opción de instalación Server Core incluye los siguientes servicios de rol.

| Role                                  | Servicio de rol                                                   | Nombre                    | ¿Está instalado de forma predeterminada? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servicios de certificados de Active Directory | Entidad de certificación                                        | ADCS-Cert-Authority     | N                     |
|                                       | Servicio web de directivas de inscripción de certificados                      | ADCS-ENROLL-web-Pol     | N                     |
|                                       | Servicio web de inscripción de certificados                             | ADCS-ENROLL-web-SVC     | N                     |
|                                       | Inscripción web de entidad de certificación                         | ADCS-web-inscripción     | N                     |
|                                       | Servicio de inscripción de dispositivos de red                              | ADCS-Device-Enrollment  | N                     |
|                                       | Servicio de respuesta en línea                                               | ADCS-Online-Cert        | N                     |
| Active Directory Rights Management    | Servidor de Active Directory Rights Management                      | ADRMS: servidor            | N                     |
|                                       | Compatibilidad con la federación de identidades                                    | ADRMS: identidad          | N                     |
| Servicios de archivos y almacenamiento             | Servicios de iSCSI y archivo                                        | Servicios de archivo           | N                     |
|                                       | Servidor de archivos                                                    | FS-fileserver           | N                     |
|                                       | BranchCache para archivos de red                                  | FS-BranchCache          | N                     |
|                                       | Desduplicación de datos                                             | FS: desduplicación de datos   | N                     |
|                                       | Espacios de nombres DFS                                                 | FS-DFS-Namespace        | N                     |
|                                       | Replicación DFS                                                | FS-DFS-Replication      | N                     |
|                                       | File Server Resource Manager                                   | FS-Resource-Manager     | N                     |
|                                       | Servicio del agente VSS del servidor de archivos                                  | FS-VSS-Agent            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget: servidor      | N                     |
|                                       | Proveedor de almacenamiento del destino iSCSI (proveedores de hardware de VDS y VSS) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | FS-NFS-servicio          | N                     |
|                                       | Carpetas de trabajo                                                   | FS-SyncShareService     | N                     |
|                                       | Servicios de almacenamiento                                               | Servicios de almacenamiento        | Y                     |
| Servicios de impresión y documentos           | Servidor de impresión                                                   | Print-Server            | N                     |
|                                       | Servicio LPD                                                    | Print-LPD-Service       | N                     |
| Acceso remoto                         | DirectAccess y VPN (RAS)                                     | DirectAccess: VPN        | N                     |
|                                       | Enrutamiento                                                        | Enrutamiento                 | N                     |
|                                       | Proxy de aplicación web                                          | Proxy de aplicación Web   | N                     |
| Servicios de Escritorio remoto               | Agente de conexión a Escritorio remoto                               | RDS-conexión-agente   | N                     |
|                                       | Administración de licencias de Escritorio remoto                                       | Licencias de RDS           | N                     |
|                                       | Host de virtualización de escritorio remoto                             | Virtualización de RDS      | N                     |
| Servidor web (IIS)                      | Servidor Web                                                     | Web-WebServer           | N                     |
|                                       | Características HTTP comunes                                           | Web-Common-Http         | N                     |
|                                       | Documento predeterminado                                               | Web-Default-Doc         | N                     |
|                                       | Examen de directorios                                             | Web-Dir-Browsing        | N                     |
|                                       | Errores HTTP                                                    | Web-Http-Errors         | N                     |
|                                       | Contenido estático                                                 | Web-Static-Content      | N                     |
|                                       | Redireccionamiento HTTP                                               | Web-Http-Redirect       | N                     |
|                                       | Publicación en WebDAV                                              | Publicación en Web-DAV      | N                     |
|                                       | Estado y diagnóstico                                         | Web-Health              | N                     |
|                                       | Registrar HTTP                                                   | Web-Http-Logging        | N                     |
|                                       | Registro personalizado                                                 | Web-Custom-Logging      | N                     |
|                                       | Herramientas de registro                                                  | Web-Log-Libraries       | N                     |
|                                       | Registro ODBC                                                   | Web-ODBC-Logging        | N                     |
|                                       | Monitor de solicitudes                                              | Web-Request-Monitor     | N                     |
|                                       | Seguimiento                                                        | Web-Http-Tracing        | N                     |
|                                       | Rendimiento                                                    | Web-Performance         | N                     |
|                                       | Compresión de contenido estático                                     | Web-Stat-Compression    | N                     |
|                                       | compresión de contenido dinámico                                    | Web-Dyn-Compression     | N                     |
|                                       | Seguridad                                                       | Web-Security            | N                     |
|                                       | Filtro de solicitudes                                              | Web-Filtering           | N                     |
|                                       | Autenticación básica                                           | Web-Basic-Auth          | N                     |
|                                       | compatibilidad centralizada con los certificados SSL                            | Web-CertProvider        | N                     |
|                                       | Autenticación de asignaciones de certificados de clientes                      | Web-Client-Auth         | N                     |
|                                       | Autenticación implícita                                          | Web-Digest-Auth         | N                     |
|                                       | Autenticación de asignaciones de certificados de cliente IIS                  | Web-Cert-Auth           | N                     |
|                                       | Restricciones de IP y dominio                                     | Web-IP-Security         | N                     |
|                                       | Autorización de URL                                              | Web-Url-Auth            | N                     |
|                                       | Autenticación de Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Desarrollo de aplicaciones                                        | Web-App-dev             | N                     |
|                                       | Extensibilidad de .NET                                         | Web-Net-Ext             | N                     |
|                                       | Extensibilidad de .NET 4,6                                         | Web-net-Ext45           | N                     |
|                                       | Inicialización de aplicaciones                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4,6                                                    | Web-ASP-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensiones ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | Filtros de ISAPI                                                  | Web-ISAPI-Filter        | N                     |
|                                       | Inclusión del servidor (SSI)                                           | Web-Includes            | N                     |
|                                       | Protocolo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Web-Ftp-Server          | N                     |
|                                       | Servicio FTP                                                    | Servicio Web-FTP-         | N                     |
|                                       | Extensibilidad de FTP                                              | Web-FTP-ext             | N                     |
|                                       | Herramientas de administración                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilidad con la administración de IIS 6                                 | Web-Mgmt-Compat         | N                     |
|                                       | Compatibilidad con la metabase de IIS 6                                   | Web-Metabase            | N                     |
|                                       | Herramientas de scripting de IIS 6                                          | Web-Lgcy-Scripting      | N                     |
|                                       | Compatibilidad con WMI de IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts y herramientas de administración de IIS                               | Web-Scripting-Tools     | N                     |
|                                       | Servicio de administración                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Conectividad WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Servicios WSUS                                                  | UpdateServices-servicios | N                     |
|                                       | Conectividad SQL Server                                        | UpdateServices-base de       | N                     |

## <a name="features-included-in-server-core"></a>Características incluidas en Server Core
La opción de instalación Server Core incluye las siguientes características.

| Característica                                                | Nombre                               | ¿Está instalado de forma predeterminada? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Características de .NET Framework 3.5                            | Características de .NET Framework             | N                     |
| .NET Framework 3,5 (incluye .NET 2,0 y 3,0)       | NET-Framework-Core                 | (se ha quitado)             |
| Activación HTTP                                        | NET-HTTP-Activation                | N                     |
| Activación no HTTP                                    | NET-Non-HTTP-Activ                 | N                     |
| .NET Framework características 4,6                            | NET-Framework-45-características          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4,6                                            | NET-Framework-45-ASPNET            | N                     |
| WCF Services                                           | NET-WCF-Services45                 | Y                     |
| Activación HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Activación de Message Queuing (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activación de canalización con nombre                                  | NET-WCF-Pipe-Activation45          | N                     |
| Activación de TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Uso compartido de puertos TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Servicio de transferencia inteligente en segundo plano (BITS)         | BITS                               | N                     |
| Servidor compacto                                         | BITS-compacto-servidor                | N                     |
| Cifrado BitLocker de unidades                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente para NFS                                         | Cliente NFS                         | N                     |
| Contenedores                                             | Contenedores                         | N                     |
| Protocolo de puente del centro de datos                                   | Puente del centro de datos               | N                     |
| Almacenamiento mejorado                                       | EnhancedStorage                    | N                     |
| Clústeres de conmutación por error                                    | Failover-Clustering                | N                     |
| Administración de directivas de grupo                                | GPMC                               | N                     |
| Calidad de servicio de E/S                                 | Desmontaje-QoS                         | N                     |
| Núcleo de web hospedable de IIS                                  | Web-WHC                            | N                     |
| Servidor de Administración de direcciones IP (IPAM)                    | IPAM                               | N                     |
| Servicio de servidor iSNS                                    | ISNS                               | N                     |
| Extensión IIS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-media-base            | N                     |
| Message Queue Server                                        | MSMQ                               | N                     |
| Servicios de Message Queue Server                               | MSMQ-Services                      | N                     |
| Message Queuing Server                                 | MSMQ-Server                        | N                     |
| Integración del servicio de directorio                          | MSMQ-Directory                     | N                     |
| Compatibilidad con HTTP                                           | MSMQ-HTTP-Support                  | N                     |
| Desencadenadores de Message Queue Server                               | MSMQ-Triggers                      | N                     |
| Servicio de enrutamiento                                        | MSMQ-Routing                       | N                     |
| Proxy DCOM de Message Queue Server                             | MSMQ-DCOM                          | N                     |
| E/S de múltiples rutas                                          | Multipath-IO                       | N                     |
| MultiPoint Connector                                   | Conector Multipoint               | N                     |
| Servicios de Multipoint Connector                          | Multipoint-Connector-Services      | N                     |
| Multipoint Manager y Multipoint Dashboard            | Multipoint-Tools                   | N                     |
| Equilibrio de carga de red                                 | NLB                                | N                     |
| Protocolo de resolución de nombres de mismo nivel                          | PNRP                               | N                     |
| Experiencia de calidad de audio y vídeo de Windows (qWave)                 | qWave                              | N                     |
| Compresión diferencial remota                        | RDC                                | N                     |
| Herramientas de administración remota del servidor                     | RSAT                               | N                     |
| Herramientas de administración de características                           | RSAT-Feature-Tools                 | N                     |
| Utilidades de administración de Cifrado de unidad BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Herramientas de LLDP DataCenterBridging                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Herramientas de clústeres de conmutación por error                              | RSAT-Clustering                    | N                     |
| Módulo de clúster de conmutación por error para Windows PowerShell         | RSAT: agrupación en clústeres-PowerShell         | N                     |
| Servidor de automatización de clústeres de conmutación por error                     | RSAT-clustering-AutomationServer   | N                     |
| Interfaz de comandos de clúster de conmutación por error                     | RSAT-clustering-CmdInterface       | N                     |
| Cliente de administración de direcciones IP (IPAM)                    | IPAM-cliente-característica                | N                     |
| Herramientas de máquinas virtuales blindadas                                      | RSAT-Blindated-VM-Tools             | N                     |
| Módulo de réplica de almacenamiento para Windows PowerShell          | RSAT: Storage-Replica               | N                     |
| Herramientas de administración de roles                              | RSAT-Role-Tools                    | N                     |
| Herramientas de AD DS y AD LDS                                 | RSAT: herramientas de AD                      | N                     |
| Módulo de Active Directory para Windows PowerShell         | RSAT: AD-PowerShell                 | N                     |
| Herramientas de AD DS                                            | RSAT-ADDS                          | N                     |
| Centro de administración de Active Directory                 | RSAT: AD-AdminCenter                | N                     |
| Complementos y herramientas de línea de comandos de AD DS                  | RSAT: agrega herramientas                    | N                     |
| Complementos y herramientas de línea de comandos de AD LDS                 | RSAT-ADLDS                         | N                     |
| Herramientas de administración de Hyper-V                               | RSAT: Hyper-V-Tools                 | N                     |
| Módulo de Hyper-V para Windows PowerShell                  | Hyper-V: PowerShell                 | N                     |
| Herramientas de Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| API y cmdlets de PowerShell                             | UpdateServices-API                 | N                     |
| Herramientas del servidor DHCP                                      | RSAT-DHCP                          | N                     |
| Herramientas del servidor DNS                                       | RSAT-DNS-Server                    | N                     |
| Herramientas de administración de acceso remoto                         | RSAT-RemoteAccess                  | N                     |
| Módulo de acceso remoto para Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| Proxy RPC sobre HTTP                                    | Proxy RPC a través de HTTP                | N                     |
| Recopilación de eventos de configuración y arranque                        | Setup-and-boot-Event-Collection    | N                     |
| Servicios simples de TCP/IP                                 | Simple-TCPIP                       | N                     |
| Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS                      | FS-SMB1                            | Y                     |
| Límite de ancho de banda SMB                                    | FS-SMBBW                           | N                     |
| Servicio SNMP                                           | SNMP-Service                       | N                     |
| Proveedor WMI de SNMP                                      | SNMP-WMI-Provider                  | N                     |
| Cliente Telnet                                          | Telnet-Client                      | N                     |
| Herramientas de blindaje de máquinas virtuales para la administración de tejidos               | FabricShieldedTools                | N                     |
| Características de Windows Defender                              | Características de Windows-Defender          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows Internal Database                              | Windows-Internal-Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Motor 2,0 de Windows PowerShell                          | PowerShell-V2                      | (se ha quitado)             |
| Servicio de configuración de estado deseado de Windows PowerShell | Servicio DSC                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Servicio de activación de procesos de Windows                     | WAS                                | N                     |
| Modelo de proceso                                          | WAS-Process-Model                  | N                     |
| Entorno de .NET 3,5                                   | WAS-NET-Environment                | N                     |
| API de configuración                                     | WAS-Config-APIs                    | N                     |
| Copias de seguridad de Windows Server                                  | Copias de seguridad de Windows-Server              | N                     |
| Herramientas de migración de Windows Server                         | Migración                          | N                     |
| Administración de almacenamiento basada en estándares de Windows             | WindowsStorageManagementService    | N                     |
| Extensión WinRM de IIS                                    | WinRM-IIS-ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Compatibilidad con WoW64                                          | Compatibilidad con WoW64                      | Y                     |
|                                                        |                                    |                       |
