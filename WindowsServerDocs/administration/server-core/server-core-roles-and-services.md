---
title: Roles, servicios de rol y características incluidas en Windows Server - Server Core
description: ¿Qué roles y características se incluyen en la opción de instalación Server Core de Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859296"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Roles, servicios de rol y características incluidas en Windows Server - Server Core

> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

Generalmente hablamos [lo del *no* en Server Core](server-core-removed-roles.md) : ahora, vamos a intentar un enfoque diferente y le indicará qué del *incluye* y si algo es *instala de forma predeterminada*. Los siguientes roles, servicios de rol y características son *en* la opción de instalación Server Core de Windows Server. Utilice esta información para ayudar a averiguar si la opción Server Core funciona para su entorno. Porque se trata de una lista grande, considere la posibilidad de buscar el rol o característica específicos que le interesa - si esa búsqueda no devuelve lo está buscando, no se incluye en Server Core.

Por ejemplo, si busca "Host de sesión de escritorio remoto:" no encontrará en esta página. Eso es porque el Host de sesión de escritorio remoto no está incluido en la imagen de Server Core.

Recuerde que puede [siempre RECURRO](server-core-removed-roles.md) a qué del *no* incluido. Se trata de una manera diferente de mirar las cosas.

## <a name="roles-included-in-server-core"></a>Roles de Server Core
La opción de instalación Server Core incluye los siguientes roles de servidor.

| Rol                                            | Nombre                           | ¿Se instala de forma predeterminada? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servicios de certificados de Active Directory           | AD-Certificate                 | N                     |
| Active Directory Domain Services                | Servicios de dominio de AD             | N                     |
| Servicios de federación de Active Directory (AD FS)            | ADFS-Federation                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Certificación de estado del dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Servicios de archivos y almacenamiento                       | FileAndStorage-Services        | esté                     |
| Servicio de protección de host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Servicios de impresión y documentos                     | Servicios de impresión                 | N                     |
| Acceso remoto                                   | RemoteAccess                   | N                     |
| Servicios de Escritorio remoto                         | Servicios de escritorio remoto        | N                     |
| Volume Activation Services                      | VolumeActivation               | N                     |
| Servidor Web IIS                                  | Servidor Web                     | N                     |
| Experiencia con Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Servicios de rol incluidos en Server Core
La opción de instalación Server Core incluye los siguientes servicios de rol.

| Rol                                  | Servicio de rol                                                   | Nombre                    | ¿Se instala de forma predeterminada? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servicios de certificados de Active Directory | Entidad de certificación                                        | ADCS-Cert-Authority     | N                     |
|                                       | Servicio web de directivas de inscripción de certificados                      | ADCS-Enroll-Web-Pol     | N                     |
|                                       | Servicio web de inscripción de certificados                             | ADCS-Enroll-Web-Svc     | N                     |
|                                       | Inscripción web de entidad de certificación                         | ADCS-Web-Enrollment     | N                     |
|                                       | Servicio de inscripción de dispositivos de red                              | Inscripción de dispositivos de ADCS  | N                     |
|                                       | Servicio de respuesta en línea                                               | ADCS-Online-Cert        | N                     |
| Active Directory Rights Management    | Servidor de Active Directory Rights Management                      | Servidor de ADRMS            | N                     |
|                                       | Compatibilidad con la federación de identidades                                    | Identidad de ADRMS          | N                     |
| Servicios de archivos y almacenamiento             | Servicios de iSCSI y archivo                                        | Servicios de archivo           | N                     |
|                                       | Servidor de archivos                                                    | FS-FileServer           | N                     |
|                                       | BranchCache para archivos de red                                  | FS-BranchCache          | N                     |
|                                       | Desduplicación de datos                                             | FS-Data-Deduplication   | N                     |
|                                       | Espacios de nombres DFS                                                 | DFS-FS-Namespace        | N                     |
|                                       | Replicación DFS                                                | FS de la replicación DFS      | N                     |
|                                       | Administrador de recursos del servidor de archivos                                   | FS-Resource-Manager     | N                     |
|                                       | Servicio del agente VSS del servidor de archivos                                  | FS-VSS-Agent            | N                     |
|                                       | Servidor de destino iSCSI                                            | iSCSITarget-Server      | N                     |
|                                       | Proveedor de almacenamiento (proveedores de hardware de VDS y VSS) de destino iSCSI | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | FS-NFS-Service          | N                     |
|                                       | Carpetas de trabajo                                                   | FS-SyncShareService     | N                     |
|                                       | Servicios de almacenamiento                                               | Storage-Services        | esté                     |
| Servicios de impresión y documentos           | Servidor de impresión                                                   | Servidor de impresión            | N                     |
|                                       | Servicio LPD                                                    | Print-LPD-Service       | N                     |
| Acceso remoto                         | DirectAccess y VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Enrutamiento                                                        | Enrutamiento                 | N                     |
|                                       | Proxy de aplicación web                                          | Web-Application-Proxy   | N                     |
| Servicios de Escritorio remoto               | Agente de conexión a Escritorio remoto                               | RDS-Connection-Broker   | N                     |
|                                       | Administración de licencias de Escritorio remoto                                       | Licencias de RDS           | N                     |
|                                       | Host de virtualización de escritorio remoto                             | Virtualización de RDS      | N                     |
| Servidor web (IIS)                      | Web Server                                                     | Web-WebServer           | N                     |
|                                       | Características HTTP comunes                                           | Web-Common-Http         | N                     |
|                                       | Documento predeterminado                                               | Web-Default-Doc         | N                     |
|                                       | Examen de directorios                                             | Exploración Web-Dir        | N                     |
|                                       | Errores HTTP                                                    | Web-Http-Errors         | N                     |
|                                       | Contenido estático                                                 | Web-Static-Content      | N                     |
|                                       | Redirección HTTP                                               | Web-Http-Redirect       | N                     |
|                                       | Publicación en WebDAV                                              | Web-DAV-Publishing      | N                     |
|                                       | Estado y diagnóstico                                         | Web-Health              | N                     |
|                                       | Registro HTTP                                                   | Web-Http-Logging        | N                     |
|                                       | Registro personalizado                                                 | Registro de Web personalizada      | N                     |
|                                       | Herramientas de registro                                                  | Bibliotecas de registro Web       | N                     |
|                                       | Registro ODBC                                                   | Web-ODBC-Logging        | N                     |
|                                       | Monitor de solicitudes                                              | Web-Request-Monitor     | N                     |
|                                       | Seguimiento                                                        | Web-Http-Tracing        | N                     |
|                                       | Rendimiento                                                    | Rendimiento Web         | N                     |
|                                       | Compresión de contenido estático                                     | Web-Stat-Compression    | N                     |
|                                       | Compresión de contenido dinámico                                    | Web-Dyn-Compression     | N                     |
|                                       | Seguridad                                                       | Seguridad en la Web            | N                     |
|                                       | Request Filtering                                              | Filtrado de Web           | N                     |
|                                       | Autenticación básica                                           | Web-Basic-Auth          | N                     |
|                                       | Compatibilidad con certificados SSL centralizado                            | Web-CertProvider        | N                     |
|                                       | Autenticación de asignaciones de certificado de cliente                      | Web-Client-Auth         | N                     |
|                                       | Autenticación implícita                                          | Web-Digest-Auth         | N                     |
|                                       | Autenticación de asignaciones de certificado de cliente IIS                  | Web-Cert-Auth           | N                     |
|                                       | Restricciones de IP y dominio                                     | IP-Web-Security         | N                     |
|                                       | Autorización de URL                                              | Web-Url-Auth            | N                     |
|                                       | Autenticación de Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Desarrollo de aplicaciones                                        | Web-App-Dev             | N                     |
|                                       | Extensibilidad de .NET 3.5                                         | Web-Net-Ext             | N                     |
|                                       | Extensibilidad de .NET 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | Inicialización de aplicaciones                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensiones ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-Filter        | N                     |
|                                       | Inclusión del servidor (SSI)                                           | Web incluye            | N                     |
|                                       | Protocolo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Web-Ftp-Server          | N                     |
|                                       | Servicio FTP                                                    | Web-Ftp-Service         | N                     |
|                                       | Extensibilidad de FTP                                              | Web-Ftp-Ext             | N                     |
|                                       | Herramientas de administración                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilidad con la administración de IIS 6                                 | Web-Mgmt-Compat         | N                     |
|                                       | Compatibilidad con la metabase de IIS 6                                   | Metabase de Web            | N                     |
|                                       | Herramientas de Scripting 6 de IIS                                          | Web-Lgcy-Scripting      | N                     |
|                                       | Compatibilidad con WMI de IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts y herramientas de administración de IIS                               | Herramientas Web de Scripting     | N                     |
|                                       | Servicio de administración                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Conectividad WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Servicios WSUS                                                  | UpdateServices-Services | N                     |
|                                       | Conectividad de SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Características incluidas en Server Core
La opción de instalación Server Core incluye las siguientes características.

| Característica                                                | Name                               | ¿Se instala de forma predeterminada? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Características de .NET framework 3.5                            | Características de NET Framework             | N                     |
| .NET framework 3.5 (incluye .NET 2.0 y 3.0)       | NET-Framework-Core                 | (quitar)             |
| Activación de HTTP                                        | NET-HTTP-Activation                | N                     |
| Activación que no sea HTTP                                    | NET-Non-HTTP-Activ                 | N                     |
| Características de .NET framework 4.6                            | NET-Framework-45-características          | esté                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | esté                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| Servicios de WCF                                           | NET-WCF-Services45                 | esté                     |
| Activación de HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Activación de Message Queuing (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activación de canalización con nombre                                  | NET-WCF-Pipe-Activation45          | N                     |
| Activación de TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Uso compartido de puertos TCP                                       | NET-WCF-TCP-PortSharing45          | esté                     |
| Servicio de transferencia inteligente en segundo plano (BITS)         | BITS                               | N                     |
| Servidor compacto                                         | BITS-Compact-Server                | N                     |
| Cifrado de unidad BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente para NFS                                         | NFS-Client                         | N                     |
| Contenedores                                             | Contenedores                         | N                     |
| Protocolo de puente del centro de datos                                   | Data-Center-Bridging               | N                     |
| Almacenamiento mejorado                                       | EnhancedStorage                    | N                     |
| Clústeres de conmutación por error                                    | Agrupación en clústeres de conmutación por error                | N                     |
| Administración de directivas de grupo                                | GPMC                               | N                     |
| Calidad de servicio de E/S                                 | DiskIo-QoS                         | N                     |
| Núcleo de web hospedable de IIS                                  | Web-WHC                            | N                     |
| Servidor de Administración de direcciones IP (IPAM)                    | IPAM                               | N                     |
| Servicio de servidor iSNS                                    | ISNS                               | N                     |
| Extensión IIS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-Media-Foundation            | N                     |
| Message Queue Server                                        | MSMQ                               | N                     |
| Servicios de Message Queue Server                               | MSMQ-Services                      | N                     |
| Message Queue Server                                 | MSMQ-Server                        | N                     |
| Integración del servicio de directorio                          | MSMQ-Directory                     | N                     |
| Compatibilidad con HTTP                                           | MSMQ-HTTP-Support                  | N                     |
| Desencadenadores de Message Queue Server                               | MSMQ-Triggers                      | N                     |
| Servicio de enrutamiento                                        | Enrutamiento de MSMQ                       | N                     |
| Proxy DCOM de Message Queue Server                             | MSMQ-DCOM                          | N                     |
| E/S de múltiples rutas                                          | Multipath-IO                       | N                     |
| MultiPoint Connector                                   | MultiPoint-Connector               | N                     |
| Conector de multiPoint Services                          | MultiPoint-Connector-Services      | N                     |
| MultiPoint Manager y el panel de MultiPoint            | MultiPoint-Tools                   | N                     |
| Equilibrio de carga de red                                 | NLB                                | N                     |
| Protocolo de resolución de nombres de mismo nivel                          | PNRP                               | N                     |
| Windows Audio Video Experience (qWAVE)                 | qWave                              | N                     |
| Compresión diferencial remota                        | RDC                                | N                     |
| Herramientas de administración remota del servidor                     | RSAT                               | N                     |
| Herramientas de administración de características                           | RSAT-Feature-Tools                 | N                     |
| Utilidades de administración de cifrado de unidad BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| LLDP de DataCenterBridging                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Herramientas de clústeres de conmutación por error                              | RSAT-Clustering                    | N                     |
| Módulo de clúster de conmutación por error de Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Servidor de automatización del clúster de conmutación por error                     | RSAT-Clustering-AutomationServer   | N                     |
| Interfaz de comandos de clúster de conmutación por error                     | RSAT-Clustering-CmdInterface       | N                     |
| Cliente de administración (IPAM) dirección IP                    | IPAM-Client-Feature                | N                     |
| Herramientas de la máquina virtual blindada                                      | RSAT-blindadas-VM-Tools             | N                     |
| Módulo de réplica de almacenamiento para Windows PowerShell          | RSAT-Storage-Replica               | N                     |
| Herramientas de administración de roles                              | RSAT-Role-Tools                    | N                     |
| Herramientas de AD DS y AD LDS                                 | RSAT-AD-Tools                      | N                     |
| Módulo de Active Directory para Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Herramientas de AD DS                                            | RSAT-ADDS                          | N                     |
| Centro de administración de Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Complementos y herramientas de línea de comandos de AD DS                  | RSAT-ADDS-Tools                    | N                     |
| Herramientas de línea de comandos y complementos de AD LDS                 | RSAT-ADLDS                         | N                     |
| Herramientas de administración de Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Módulo de Hyper-V para Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Herramientas de Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| Cmdlets de PowerShell y API                             | UpdateServices-API                 | N                     |
| Herramientas del servidor DHCP                                      | RSAT-DHCP                          | N                     |
| Herramientas del servidor DNS                                       | RSAT-DNS-Server                    | N                     |
| Herramientas de administración de acceso remoto                         | RSAT-RemoteAccess                  | N                     |
| Módulo de acceso remoto para Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| Proxy RPC sobre HTTP                                    | RPC-over-HTTP-Proxy                | N                     |
| Recopilación de eventos de configuración y arranque                        | Setup-and-Boot-Event-Collection    | N                     |
| Servicios simples de TCP/IP                                 | TCP/IP simple                       | N                     |
| Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS                      | FS-SMB1                            | esté                     |
| Límite de ancho de banda SMB                                    | FS-SMBBW                           | N                     |
| Servicio SNMP                                           | SNMP-Service                       | N                     |
| Proveedor WMI de SNMP                                      | SNMP-WMI-Provider                  | N                     |
| Cliente Telnet                                          | Telnet-Client                      | N                     |
| Herramientas de blindaje de máquinas virtuales para la administración de tejidos               | FabricShieldedTools                | N                     |
| Características de Windows Defender                              | Características de Windows Defender          | esté                     |
| Windows Defender                                       | Windows Defender                   | esté                     |
| Windows Internal Database                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | esté                     |
| Windows PowerShell 5.1                                 | PowerShell                         | esté                     |
| Motor de Windows PowerShell 2.0                          | PowerShell-V2                      | (quitar)             |
| Windows PowerShell Desired State Configuration de servicio | DSC-Service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Servicio de activación de procesos de Windows                     | FUE                                | N                     |
| Modelo de proceso                                          | Modelo de proceso se ha                  | N                     |
| Entorno de .NET 3.5                                   | Entorno de red se ha                | N                     |
| API de configuración                                     | WAS-Config-APIs                    | N                     |
| Copias de seguridad de Windows Server                                  | Windows-Server-Backup              | N                     |
| Herramientas de migración de Windows Server                         | Migración                          | N                     |
| Administración de almacenamiento basada en estándares de Windows             | WindowsStorageManagementService    | N                     |
| Extensión IIS de WinRM                                    | WinRM-IIS-Ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Compatibilidad con WoW64                                          | WoW64-Support                      | esté                     |
|                                                        |                                    |                       |
