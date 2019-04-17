---
title: 'Roles, servicios de rol y características que se incluyen en Windows Server: Server Core'
description: ¿Qué funciones y características se incluyen en la opción de instalación Server Core de Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 439edac95e1427086268cab469ed03b94db630da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "2217745"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Roles, servicios de rol y características que se incluyen en Windows Server: Server Core

> Se aplica a: (delimitadas anuales del canal) de Windows Server y Windows Server 2016

Por lo general hablamos de [¿qué del *no* en Server Core](server-core-removed-roles.md) - ahora vamos a intentar un enfoque diferente y saber lo que se *incluyen* y si algo *instalados de forma predeterminada*. Los siguientes roles, servicios de rol y características se encuentran *en* la opción de instalación Server Core de Windows Server. Use esta información para ayudar a averiguar si la opción Server Core funciona para su entorno. Puesto que se trata de una lista grande, considere la posibilidad de buscando el rol específico o una característica que le interesa - si la búsqueda no devuelve lo está buscando, no se incluye en Server Core.

Por ejemplo, si busca "Host de sesión de escritorio remoto -" que no encontrará en esta página. Esto es debido a que el Host de sesión de escritorio remoto no está incluido en la imagen de Server Core.

Recuerde que puede [tener un aspecto siempre](server-core-removed-roles.md) *en lo que se incluye* . Se trata de una manera diferente de mirar cosas.

## <a name="roles-included-in-server-core"></a>Funciones incluidas en Server Core
La opción de instalación Server Core incluye las siguientes funciones de servidor.

| Rol                                            | Nombre                           | ¿Se instala de forma predeterminada? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servicios de certificados de Active Directory           | Certificado de AD                 | N                     |
| Servicios de dominio de ActiveDirectory                | Servicios de dominio de AD             | N                     |
| Servicios de federación de Active Directory (AD FS)            | Federación de AD FS                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Atestación de estado del dispositivo                       | DeviceHealthAttestationService | N                     |
| Servidor DHCP                                     | DHCP                           | N                     |
| Servidor DNS                                      | DNS                            | N                     |
| Servicios de archivos y almacenamiento                       | Servicios de FileAndStorage        | Y                     |
| Servicio de protección de host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Servicios de impresión y documentos                     | Servicios de impresión                 | N                     |
| Acceso remoto                                   | Acceso remoto                   | N                     |
| Servicios de Escritorio remoto                         | Servicios de escritorio remoto        | N                     |
| Volume Activation Services                      | VolumeActivation               | N                     |
| Servidor Web de IIS                                  | Servidor Web                     | N                     |
| Experiencia con WindowsServer Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Servicios de rol que se incluyen en Server Core
La opción de instalación Server Core incluye los siguientes servicios de rol.

| Rol                                  | Servicio de rol                                                   | Nombre                    | ¿Se instala de forma predeterminada? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servicios de certificados de Active Directory | Entidad de certificación                                        | Entidad de Cert ADC     | N                     |
|                                       | Servicio Web de directiva de inscripción de certificados                      | ADC-inscribirse-Web-Pol     | N                     |
|                                       | Servicio Web de inscripción de certificados                             | ADC-inscribirse-Web-Svc     | N                     |
|                                       | Inscripción Web de entidad de certificación                         | ADC inscripción en Web     | N                     |
|                                       | Servicio de inscripción de dispositivos de red                              | Dispositivo de ADC de inscripción  | N                     |
|                                       | Respondedor en línea                                               | ADC Cert en línea        | N                     |
| Active Directory Rights Management    | Servidor de Active Directory Rights Management                      | ADRMS Server            | N                     |
|                                       | Compatibilidad con la federación de identidad                                    | ADRMS-identidad          | N                     |
| Servicios de archivos y almacenamiento             | Servicios de archivo e iSCSI                                        | Servicios de archivos           | N                     |
|                                       | Servidor de archivos                                                    | FS-servidor de archivos           | N                     |
|                                       | BranchCache para archivos de red                                  | FS BranchCache          | N                     |
|                                       | Desduplicación de datos                                             | Desduplicación de datos de FS   | N                     |
|                                       | Espacios de nombres DFS                                                 | FS-DFS-Namespace        | N                     |
|                                       | Replicación DFS                                                | FS de la replicación DFS      | N                     |
|                                       | Administrador de recursos del servidor de archivos                                   | Administrador de recursos de FS     | N                     |
|                                       | Servicio del agente VSS del servidor de archivos                                  | Agente de FS-VSS            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget Server      | N                     |
|                                       | Proveedor de almacenamiento de destino (proveedores de hardware VDS y VSS) iSCSI | iSCSITarget-VSS-VDS     | N                     |
|                                       | Servidor para NFS                                                 | FS-NFS-Service          | N                     |
|                                       | Carpetas de trabajo                                                   | FS-SyncShareService     | N                     |
|                                       | Servicios de almacenamiento                                               | Servicios de almacenamiento de información        | Y                     |
| Servicios de impresión y documentos           | Servidor de impresión                                                   | Servidor de impresión            | N                     |
|                                       | Servicio LPD                                                    | Servicio de impresión LPD       | N                     |
| Acceso remoto                         | DirectAccess y VPN (RAS)                                     | DirectAccess VPN        | N                     |
|                                       | Enrutamiento                                                        | Enrutamiento                 | N                     |
|                                       | Proxy de aplicación web                                          | Proxy de aplicación Web   | N                     |
| Servicios de Escritorio remoto               | Agente de conexión a Escritorio remoto                               | Agente de conexión de RDS   | N                     |
|                                       | Administración de licencias de escritorio remoto                                       | Licencias de RDS           | N                     |
|                                       | Host de virtualización de escritorio remoto                             | Virtualización de RDS      | N                     |
| Servidor web (IIS)                      | Servidor web                                                     | WebServer de Web           | N                     |
|                                       | Características HTTP comunes                                           | Web-común-Http         | N                     |
|                                       | Documento predeterminado                                               | Web-predeterminado-Doc         | N                     |
|                                       | Examen de directorios                                             | Exploración Web-Dir        | N                     |
|                                       | Errores HTTP                                                    | Errores de Http de Web         | N                     |
|                                       | Contenido estático                                                 | Contenido estático-Web      | N                     |
|                                       | Redirección HTTP                                               | Redirección de Http de Web       | N                     |
|                                       | Publicación de WebDAV                                              | Publicación en Web-DAV      | N                     |
|                                       | Estado y diagnóstico                                         | Mantenimiento de la Web              | N                     |
|                                       | Registro HTTP                                                   | Registro de Http de Web        | N                     |
|                                       | Registro personalizado                                                 | Registro de Web personalizado      | N                     |
|                                       | Herramientas de registro                                                  | Bibliotecas de registro de Web       | N                     |
|                                       | Registro de ODBC                                                   | Registro de ODBC Web        | N                     |
|                                       | Monitor de solicitudes                                              | Monitor de solicitudes Web     | N                     |
|                                       | Seguimiento                                                        | Seguimiento de Web Http        | N                     |
|                                       | Rendimiento                                                    | Rendimiento de Web         | N                     |
|                                       | Compresión de contenido estático                                     | Compresión de Stat Web    | N                     |
|                                       | Compresión de contenido dinámico                                    | Compresión de Dyn Web     | N                     |
|                                       | Seguridad                                                       | Seguridad en la Web            | N                     |
|                                       | Filtrado de solicitudes                                              | Filtrado de Web           | N                     |
|                                       | Autenticación básica                                           | Web-Basic-Auth          | N                     |
|                                       | Compatibilidad con los certificados SSL centralizado                            | Web CertProvider        | N                     |
|                                       | Autenticación de asignación de certificados de cliente                      | Autenticación de cliente Web         | N                     |
|                                       | Autenticación implícita                                          | Autenticación de texto implícita de Web         | N                     |
|                                       | Autenticación de asignación de certificados de cliente IIS                  | Web-Cert-Auth           | N                     |
|                                       | Dirección IP y las restricciones de dominio                                     | Seguridad de IP de Web         | N                     |
|                                       | Autorización de dirección URL                                              | Dirección Url-Web-Auth            | N                     |
|                                       | Autenticación de Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Desarrollo de aplicaciones                                        | Desarrollo de aplicaciones Web             | N                     |
|                                       | Extensibilidad de .NET 3.5                                         | Web-Net-Ext             | N                     |
|                                       | Extensibilidad de .NET 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | Inicialización de la aplicación                                     | Web AppInit             | N                     |
|                                       | ASP                                                            | Web ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web ASP.NET             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensiones ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | Filtros ISAPI                                                  | Filtro de ISAPI de Web        | N                     |
|                                       | Inclusión del servidor                                           | Incluye Web            | N                     |
|                                       | Protocolo WebSocket                                             | Web WebSockets          | N                     |
|                                       | Servidor FTP                                                     | Servidor de Ftp de Web          | N                     |
|                                       | Servicio FTP                                                    | Servicio Web de Ftp         | N                     |
|                                       | Extensibilidad FTP                                              | Web-Ftp-Ext             | N                     |
|                                       | Herramientas de administración                                               | Herramientas de administración de Web          | N                     |
|                                       | Compatibilidad con la administración de IIS 6                                 | Web-Mgmt-anteriores         | N                     |
|                                       | Compatibilidad con la Metabase de 6 de IIS                                   | Metabase de Web            | N                     |
|                                       | Herramientas de Scripting 6 de IIS                                          | Lgcy-secuencias de comandos Web      | N                     |
|                                       | Compatibilidad de 6 WMI de IIS                                        | Web-WMI                 | N                     |
|                                       | Las secuencias de comandos de administración de IIS y herramientas                               | Herramientas de secuencias de comandos Web     | N                     |
|                                       | Servicio de administración                                             | Servicio Web de administración        | N                     |
| Windows Server Update Services        | Conectividad WID                                               | UpdateServices WidDB    | N                     |
|                                       | Servicios WSUS                                                  | Servicios de UpdateServices | N                     |
|                                       | Conectividad de SQL Server                                        | UpdateServices DB       | N                     |

## <a name="features-included-in-server-core"></a>Características incluidas en Server Core
La opción de instalación Server Core incluye las siguientes características.

| Función                                                | Nombre                               | ¿Se instala de forma predeterminada? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Características de .NET framework 3.5                            | Características de NET Framework             | N                     |
| .NET framework 3.5 (incluye .NET 2.0 y 3.0)       | Básico de NET Framework                 | (quitar)             |
| Activación HTTP                                        | Activación de HTTP de NET                | N                     |
| Activación no HTTP                                    | NET-no-HTTP-activarla                 | N                     |
| Características de .NET framework 4.6                            | NET Framework-45-las características          | Y                     |
| .NET Framework 4.6                                     | NET Framework 45 núcleos              | Y                     |
| ASP.NET 4.6                                            | NET Framework 45 ASPNET            | N                     |
| Servicios de WCF                                           | NET-WCF-Services45                 | Y                     |
| Activación HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Activación de Message Queue Server (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activación de canalizaciones con nombre                                  | NET-WCF-barra vertical-Activation45          | N                     |
| Activación de TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Uso compartido de puertos TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Servicio de transferencia inteligente en segundo plano (BITS)         | BITS                               | N                     |
| Servidor compacto                                         | Servidor de compacto de BITS                | N                     |
| Cifrado de unidad BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Cliente para NFS                                         | Cliente NFS                         | N                     |
| Contenedores                                             | Contenedores                         | N                     |
| Protocolo de puente del centro de datos                                   | Puente de centro de datos               | N                     |
| Almacenamiento mejorado                                       | EnhancedStorage                    | N                     |
| Clúster de conmutación por error                                    | Agrupación en clústeres de conmutación por error                | N                     |
| Administración de directivas de grupo                                | GPMC                               | N                     |
| Calidad de servicio de E/S                                 | DiskIo QoS                         | N                     |
| Núcleo de web hospedable de IIS                                  | Web WHC                            | N                     |
| Dirección IP de servidor administración (IPAM)                    | IPAM                               | N                     |
| Servicio de servidor iSNS                                    | ISNS                               | N                     |
| Extensión IIS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Servidor-Media-Foundation            | N                     |
| Message Queue Server                                        | MSMQ                               | N                     |
| Servicios de Message Queuing                               | Servicios de MSMQ                      | N                     |
| Message Queue Server                                 | MSMQ-Server                        | N                     |
| Integración del servicio de directorio                          | MSMQ-Directory                     | N                     |
| Compatibilidad con HTTP                                           | Compatibilidad con MSMQ-HTTP                  | N                     |
| Desencadenadores de Message Queue Server                               | Desencadenadores de MSMQ                      | N                     |
| Servicio de enrutamiento                                        | Enrutamiento de MSMQ                       | N                     |
| Proxy Message Queue Server DCOM                             | DCOM DE MSMQ                          | N                     |
| E/S de múltiples rutas                                          | E/S de múltiples rutas                       | N                     |
| MultiPoint Connector                                   | Conector de multipunto               | N                     |
| Servicios del conector de multipunto                          | Servicios del conector de multipunto      | N                     |
| Administrador de multipunto y panel multipunto            | Herramientas de multipunto                   | N                     |
| Equilibrio de carga de red                                 | NLB                                | N                     |
| Protocolo de resolución de nombres de mismo nivel                          | PNRP                               | N                     |
| Windows Audio Video Experience (qWAVE)                 | qWave                              | N                     |
| Compresión diferencial remota                        | RDC                                | N                     |
| Herramientas de administración remota del servidor                     | RSAT                               | N                     |
| Herramientas de administración de características                           | RSAT-Feature-Tools                 | N                     |
| Utilidades de administración de cifrado de unidad BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Herramientas de DataCenterBridging LLDP                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Herramientas de agrupación en clústeres de conmutación por error                              | Agrupación en clústeres de RSAT                    | N                     |
| Módulo de clúster de conmutación por error para Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Servidor de automatización de clúster de conmutación por error                     | Agrupación en clústeres RSAT-AutomationServer   | N                     |
| Interfaz de comandos de clúster de conmutación por error                     | Agrupación en clústeres RSAT-CmdInterface       | N                     |
| Cliente de administración (IPAM) de dirección IP                    | Característica de cliente IPAM                | N                     |
| Herramientas de protección de máquina virtual                                      | RSAT-protegido frente a-VM-Tools             | N                     |
| Módulo de réplica de almacenamiento para Windows PowerShell          | Réplica de almacenamiento RSAT               | N                     |
| Herramientas de administración de funciones                              | RSAT-Role-Tools                    | N                     |
| Herramientas de AD LDS y AD DS                                 | RSAT-AD-Tools                      | N                     |
| Módulo de Active Directory para Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Herramientas de AD DS                                            | AGREGA RSAT                          | N                     |
| Centro de administración de Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Complementos de AD DS y las herramientas de línea de comandos                  | Herramientas RSAT agrega                    | N                     |
| Herramientas de línea de comandos y los complementos de AD LDS                 | RSAT-ADLDS                         | N                     |
| Herramientas de administración de Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Módulo de Hyper-V para Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Herramientas de Windows Server Update Services                   | UpdateServices RSAT                | N                     |
| Cmdlets de PowerShell y API                             | API de UpdateServices                 | N                     |
| Herramientas del servidor DHCP                                      | RSAT DHCP                          | N                     |
| Herramientas del servidor DNS                                       | RSAT-DNS-Server                    | N                     |
| Herramientas de administración de acceso remoto                         | RSAT-acceso remoto                  | N                     |
| Módulo de acceso remoto para Windows PowerShell            | RSAT-acceso remoto-PowerShell       | N                     |
| Proxy RPC sobre HTTP                                    | RPC sobre HTTP Proxy                | N                     |
| Recopilación de eventos de configuración y arranque                        | El programa de instalación-y-arranque--recopilación de eventos    | N                     |
| Servicios simples de TCP/IP                                 | Tipo simple TCPIP                       | N                     |
| Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS                      | FS-SMB1                            | Y                     |
| Límite de ancho de banda SMB                                    | FS-SMBBW                           | N                     |
| Servicio SNMP                                           | Servicio SNMP                       | N                     |
| Proveedor WMI de SNMP                                      | Proveedor de WMI de SNMP                  | N                     |
| Cliente Telnet                                          | Cliente de Telnet                      | N                     |
| Herramientas de blindaje de máquinas virtuales para la administración de tejidos               | FabricShieldedTools                | N                     |
| Características de Windows Defender                              | Características de Windows Defender          | Y                     |
| Windows Defender                                       | Windows Defender                   | Y                     |
| Windows Internal Database                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Motor de Windows PowerShell 2.0                          | PowerShell V2                      | (quitar)             |
| Windows PowerShell así lo desea el servicio de configuración de estado | DSC-Service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Servicio de activación de procesos de Windows                     | FUE                                | N                     |
| Modelo de proceso                                          | Modelo de proceso se ha                  | N                     |
| Entorno de .NET 3.5                                   | Entorno de red se ha                | N                     |
| API de configuración                                     | Se ha-Config-API                    | N                     |
| Copias de seguridad de Windows Server                                  | Windows-Server-copia de seguridad              | N                     |
| Herramientas de migración de Windows Server                         | Migración                          | N                     |
| Administración de almacenamiento basada en estándares de Windows             | WindowsStorageManagementService    | N                     |
| Extensión IIS de WinRM                                    | WinRM-IIS-Ext                      | N                     |
| Servidor WINS                                            | WINS                               | N                     |
| Compatibilidad de WoW64                                          | Compatibilidad de WoW64                      | Y                     |
|                                                        |                                    |                       |
