---
title: Roles, servicios de rol y características incluidos en Windows Server-Server Core
description: ¿Qué roles y características se incluyen en la opción de instalación Server Core de Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 2f6aed56083bd606ae2ec06b72152ef4a0461420
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476507"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Roles, servicios de rol y características incluidos en Windows Server-Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

Por lo general, hablamos de [lo que *no* está en Server Core](server-core-removed-roles.md) ; ahora vamos a probar un enfoque diferente y le indicamos lo que está *incluido* y si hay algo *instalado de forma predeterminada*. Los siguientes roles, servicios de rol y características se encuentran *en* la opción de instalación Server Core de Windows Server. Use esta información como ayuda para averiguar si la opción Server Core funciona para su entorno. Dado que se trata de una lista grande, considere la posibilidad de buscar el rol o la característica específicos que le interesen: si esa búsqueda no devuelve lo que está buscando, no se incluye en Server Core.

Por ejemplo, si busca "host de sesión de Escritorio remoto", no lo encontrará en esta página. Esto se debe a que el host de sesión de escritorio remoto no está incluido en la imagen de Server Core.

Recuerde que siempre puede [Ver](server-core-removed-roles.md) lo que *no* se incluye. Se trata simplemente de una manera diferente de ver las cosas.

## <a name="roles-included-in-server-core"></a>Roles incluidos en Server Core
La opción de instalación Server Core incluye los siguientes roles de servidor.

| Rol                                            | NOMBRE                           | ¿Está instalado de forma predeterminada? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servicios de certificados de Active Directory           | Certificado de AD                 | N                     |
| Active Directory Domain Services                | AD-Domain-Services             | N                     |
| Servicios de federación de Active Directory (AD FS)            | ADFS-Federación                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Certificación de estado del dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Servicios de archivos y almacenamiento                       | FileAndStorage-servicios        | Y                     |
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

| Rol                                  | Servicio de rol                                                   | Name                    | ¿Está instalado de forma predeterminada? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servicios de certificados de Active Directory | Entidad de certificación                                        | ADCS-CERT-Authority     | N                     |
|                                       | Servicio web de directivas de inscripción de certificados                      | ADCS-ENROLL-web-Pol     | N                     |
|                                       | Servicio web de inscripción de certificados                             | ADCS-ENROLL-web-SVC     | N                     |
|                                       | Inscripción web de entidad de certificación                         | ADCS-web-inscripción     | N                     |
|                                       | Servicio de inscripción de dispositivos de red                              | ADCS-Device-Enrollment  | N                     |
|                                       | Servicio de respuesta en línea                                               | ADCS-online-CERT        | N                     |
| Active Directory Rights Management    | Servidor de Active Directory Rights Management                      | ADRMS: servidor            | N                     |
|                                       | Compatibilidad con la federación de identidades                                    | ADRMS: identidad          | N                     |
| Servicios de archivos y almacenamiento             | Servicios de iSCSI y archivo                                        | Servicios de archivo           | N                     |
|                                       | Servidor de archivos                                                    | FS-fileserver           | N                     |
|                                       | BranchCache para archivos de red                                  | FS-BranchCache          | N                     |
|                                       | Desduplicación de datos                                             | FS: desduplicación de datos   | N                     |
|                                       | Espacios de nombres DFS                                                 | FS-DFS-namespace        | N                     |
|                                       | Replicación DFS                                                | FS-DFS-replicación      | N                     |
|                                       | Administrador de recursos del servidor de archivos                                   | FS-Resource-Manager     | N                     |
|                                       | Servicio del agente VSS del servidor de archivos                                  | FS-VSS-Agent            | N                     |
|                                       | Servidor de destino iSCSI                                            | iSCSITarget: servidor      | N                     |
|                                       | Proveedor de almacenamiento del destino iSCSI (proveedores de hardware de VDS y VSS) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | FS-NFS-servicio          | N                     |
|                                       | Carpetas de trabajo                                                   | FS-SyncShareService     | N                     |
|                                       | Servicios de almacenamiento                                               | Servicios de almacenamiento        | Y                     |
| Servicios de impresión y documentos           | Servidor de impresión                                                   | Servidor de impresión            | N                     |
|                                       | Servicio LPD                                                    | Print-LPD-Service       | N                     |
| Acceso remoto                         | DirectAccess y VPN (RAS)                                     | DirectAccess: VPN        | N                     |
|                                       | Enrutamiento                                                        | Enrutamiento                 | N                     |
|                                       | Proxy de aplicación web                                          | Proxy de aplicación Web   | N                     |
| Servicios de Escritorio remoto               | Agente de conexión a Escritorio remoto                               | RDS-conexión-agente   | N                     |
|                                       | Administración de licencias de Escritorio remoto                                       | Licencias de RDS           | N                     |
|                                       | Host de virtualización de escritorio remoto                             | Virtualización de RDS      | N                     |
| Servidor web (IIS)                      | Web Server                                                     | Web-WebServer           | N                     |
|                                       | Características HTTP comunes                                           | Web-común-http         | N                     |
|                                       | Documento predeterminado                                               | Web-default-doc         | N                     |
|                                       | Examen de directorios                                             | Web-dir-examinar        | N                     |
|                                       | Errores HTTP                                                    | Web-http-errores         | N                     |
|                                       | Contenido estático                                                 | Contenido estático en Web      | N                     |
|                                       | Redirección HTTP                                               | Web-http-redireccionamiento       | N                     |
|                                       | Publicación en WebDAV                                              | Publicación en Web-DAV      | N                     |
|                                       | Estado y diagnóstico                                         | Web-mantenimiento              | N                     |
|                                       | Registro HTTP                                                   | Web-http-registro        | N                     |
|                                       | Registro personalizado                                                 | Web: registro personalizado      | N                     |
|                                       | Herramientas de registro                                                  | Bibliotecas de registro web       | N                     |
|                                       | Registro ODBC                                                   | Web-ODBC-registro        | N                     |
|                                       | Monitor de solicitudes                                              | Web-request-monitor     | N                     |
|                                       | Seguimiento                                                        | Web-http-seguimiento        | N                     |
|                                       | Rendimiento                                                    | Rendimiento web         | N                     |
|                                       | Compresión de contenido estático                                     | Web-stat-Compression    | N                     |
|                                       | Compresión de contenido dinámico                                    | Compresión web-DYN     | N                     |
|                                       | Seguridad                                                       | Web-seguridad            | N                     |
|                                       | Request Filtering                                              | Filtrado web           | N                     |
|                                       | Autenticación básica                                           | Web-básico-autenticación          | N                     |
|                                       | Compatibilidad de certificados SSL centralizada                            | Web-CertProvider        | N                     |
|                                       | Autenticación de asignación de certificados de cliente                      | Autenticación de cliente web         | N                     |
|                                       | Autenticación implícita                                          | Web-implícita-autenticación         | N                     |
|                                       | Autenticación de asignación de certificados de cliente IIS                  | Web-CERT-auth           | N                     |
|                                       | Restricciones de IP y dominio                                     | Web-IP-seguridad         | N                     |
|                                       | Autorización de URL                                              | Autenticación Web-URL            | N                     |
|                                       | Autenticación de Windows                                         | Web-Windows-auth        | N                     |
|                                       | Desarrollo de aplicaciones                                        | Web-App-dev             | N                     |
|                                       | Extensibilidad de .NET 3,5                                         | Web-net-ext             | N                     |
|                                       | Extensibilidad de .NET 4,6                                         | Web-net-Ext45           | N                     |
|                                       | Inicialización de aplicaciones                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3,5                                                    | Web-ASP-net             | N                     |
|                                       | ASP.NET 4,6                                                    | Web-ASP-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensiones ISAPI                                               | Web-ISAPI-ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-filtro        | N                     |
|                                       | Inclusión del servidor (SSI)                                           | Web: incluye            | N                     |
|                                       | Protocolo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Web-FTP-Server          | N                     |
|                                       | Servicio FTP                                                    | Servicio Web-FTP-         | N                     |
|                                       | Extensibilidad de FTP                                              | Web-FTP-ext             | N                     |
|                                       | Herramientas de administración                                               | Herramientas de administración web          | N                     |
|                                       | Compatibilidad con la administración de IIS 6                                 | Compatibilidad de web-MGMT         | N                     |
|                                       | Compatibilidad con la metabase de IIS 6                                   | Web-metabase            | N                     |
|                                       | Herramientas de scripting de IIS 6                                          | Web-LGCY-scripting      | N                     |
|                                       | Compatibilidad con WMI de IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts y herramientas de administración de IIS                               | Herramientas de scripting Web     | N                     |
|                                       | Servicio de administración                                             | Servicio Web-MGMT-Service        | N                     |
| Windows Server Update Services        | Conectividad WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Servicios WSUS                                                  | UpdateServices-servicios | N                     |
|                                       | Conectividad SQL Server                                        | UpdateServices-base de       | N                     |

## <a name="features-included-in-server-core"></a>Características incluidas en Server Core
La opción de instalación Server Core incluye las siguientes características.

| Característica                                                | Name                               | ¿Está instalado de forma predeterminada? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| .NET Framework características 3,5                            | Características de .NET Framework             | N                     |
| .NET Framework 3,5 (incluye .NET 2,0 y 3,0)       | NET-Framework-Core                 | ha             |
| Activación de HTTP                                        | NET-HTTP-activación                | N                     |
| Activación que no sea HTTP                                    | NET-no-HTTP-Activ                 | N                     |
| .NET Framework características 4,6                            | NET-Framework-45-características          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4,6                                            | NET-Framework-45-ASPNET            | N                     |
| Servicios WCF                                           | NET-WCF-Services45                 | Y                     |
| Activación de HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Activación de Message Queuing (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activación de canalización con nombre                                  | NET-WCF-Pipe-Activation45          | N                     |
| Activación de TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Uso compartido de puertos TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Servicio de transferencia inteligente en segundo plano (BITS)         | BITS                               | N                     |
| Servidor compacto                                         | BITS-compacto-servidor                | N                     |
| Cifrado de unidad BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente para NFS                                         | Cliente NFS                         | N                     |
| Contenedores                                             | Contenedores                         | N                     |
| Protocolo de puente del centro de datos                                   | Puente del centro de datos               | N                     |
| Almacenamiento mejorado                                       | EnhancedStorage                    | N                     |
| Clústeres de conmutación por error                                    | Agrupación en clústeres de conmutación por error                | N                     |
| Administración de directivas de grupo                                | GPMC                               | N                     |
| Calidad de servicio de E/S                                 | Desmontaje-QoS                         | N                     |
| Núcleo de web hospedable de IIS                                  | Web-WHC                            | N                     |
| Servidor de Administración de direcciones IP (IPAM)                    | IPAM                               | N                     |
| Servicio de servidor iSNS                                    | ISNS                               | N                     |
| Extensión IIS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-media-base            | N                     |
| Message Queue Server                                        | MSMQ                               | N                     |
| Servicios de Message Queue Server                               | Servicios de MSMQ                      | N                     |
| Servidor de Message Queue Server                                 | MSMQ-servidor                        | N                     |
| Integración del servicio de directorio                          | Directorio MSMQ                     | N                     |
| Compatibilidad con HTTP                                           | MSMQ-HTTP-compatibilidad                  | N                     |
| Desencadenadores de Message Queue Server                               | Desencadenadores de MSMQ                      | N                     |
| Servicio de enrutamiento                                        | Enrutamiento de MSMQ                       | N                     |
| Proxy DCOM de Message Queue Server                             | MSMQ-DCOM                          | N                     |
| E/S de múltiples rutas                                          | Múltiples rutas: e/s                       | N                     |
| MultiPoint Connector                                   | Conector Multipoint               | N                     |
| Servicios de Multipoint Connector                          | Multipoint-Connector-Services      | N                     |
| Multipoint Manager y Multipoint Dashboard            | Multipoint-Tools                   | N                     |
| Equilibrio de carga de red                                 | NLB                                | N                     |
| Protocolo de resolución de nombres de mismo nivel                          | PNRP                               | N                     |
| Windows Audio Video Experience (qWAVE)                 | qWave                              | N                     |
| Compresión diferencial remota                        | RDC                                | N                     |
| Herramientas de administración remota del servidor                     | RSAT                               | N                     |
| Herramientas de administración de características                           | RSAT: herramientas de características                 | N                     |
| Utilidades de administración de Cifrado de unidad BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Herramientas de LLDP DataCenterBridging                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Herramientas de clústeres de conmutación por error                              | RSAT: agrupación en clústeres                    | N                     |
| Módulo de clúster de conmutación por error para Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Servidor de automatización de clústeres de conmutación por error                     | RSAT-clustering-AutomationServer   | N                     |
| Interfaz de comandos de clúster de conmutación por error                     | RSAT-clustering-CmdInterface       | N                     |
| Cliente de administración de direcciones IP (IPAM)                    | IPAM-cliente-característica                | N                     |
| Herramientas de máquinas virtuales blindadas                                      | RSAT-Blindated-VM-Tools             | N                     |
| Módulo de réplica de almacenamiento para Windows PowerShell          | RSAT: Storage-Replica               | N                     |
| Herramientas de administración de roles                              | RSAT: herramientas de rol                    | N                     |
| Herramientas de AD DS y AD LDS                                 | RSAT: herramientas de AD                      | N                     |
| Módulo de Active Directory para Windows PowerShell         | RSAT: AD-PowerShell                 | N                     |
| Herramientas de AD DS                                            | RSAT: AGREGA                          | N                     |
| Centro de administración de Active Directory                 | RSAT: AD-AdminCenter                | N                     |
| Complementos y herramientas de línea de comandos de AD DS                  | RSAT: agrega herramientas                    | N                     |
| Complementos y herramientas de línea de comandos de AD LDS                 | RSAT:                         | N                     |
| Herramientas de administración de Hyper-V                               | RSAT: Hyper-V-Tools                 | N                     |
| Módulo de Hyper-V para Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
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
| Servicio SNMP                                           | Servicio SNMP                       | N                     |
| Proveedor WMI de SNMP                                      | Proveedor SNMP-WMI-                  | N                     |
| Cliente Telnet                                          | Telnet-cliente                      | N                     |
| Herramientas de blindaje de máquinas virtuales para la administración de tejidos               | FabricShieldedTools                | N                     |
| Características de Windows Defender                              | Características de Windows-Defender          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Windows Internal Database                              | Windows-Internal-Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5,1                                 | PowerShell                         | Y                     |
| Motor 2,0 de Windows PowerShell                          | PowerShell-V2                      | ha             |
| Servicio de configuración de estado deseado de Windows PowerShell | Servicio DSC                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Servicio de activación de procesos de Windows                     | UTILIZADO                                | N                     |
| Modelo de proceso                                          | WAS-Process-Model                  | N                     |
| Entorno de .NET 3,5                                   | WAS-NET-entorno                | N                     |
| API de configuración                                     | WAS-config-API                    | N                     |
| Copias de seguridad de Windows Server                                  | Copias de seguridad de Windows-Server              | N                     |
| Herramientas de migración de Windows Server                         | Migración                          | N                     |
| Administración de almacenamiento basada en estándares de Windows             | WindowsStorageManagementService    | N                     |
| Extensión IIS de WinRM                                    | WinRM-IIS-ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Compatibilidad con WoW64                                          | Compatibilidad con WoW64                      | Y                     |
|                                                        |                                    |                       |
