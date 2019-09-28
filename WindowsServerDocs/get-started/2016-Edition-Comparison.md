---
title: Productos y ediciones de Windows Server 2016
description: Explica las diferencias de las ediciones Standard y Datacenter.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 01/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-a932-80cab33f419e
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: e3d32d596746d2ff137fe2517a6430976f9e77ce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391878"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2016"></a>Comparación de las ediciones Standard y Datacenter de Windows Server 2016

> Se aplica a: Windows Server 2016
  
## <a name="locks-and-limits"></a>Bloqueos y límites
|Bloqueos y límites|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|Número máximo de usuarios|Según licencias CAL|Según licencias CAL|
|Número máximo de conexiones SMB|16 777 216|16 777 216|
|Número máximo de conexiones RRAS|sin límite|sin límite|
|Número máximo de conexiones IAS|2 147 483 647|2 147 483 647|
|Número máximo de conexiones RDS|65 535|65 535|
|Número máximo de sockets de 64 bits|64|64|
|Número máximo de núcleos|sin límite|sin límite|
|RAM máxima|24 TB|24 TB|
|Puede usarse como invitado de virtualización|Sí; 2 máquinas virtuales, más un host de Hyper-V por licencia|Sí; máquinas virtuales ilimitadas, más un host de Hyper-V por licencia|
|El servidor puede unirse a un dominio|sí|sí|
|Protección de red y firewall de Edge|no|no|
|DirectAccess|sí|sí|
|Códecs de DLNA y streaming de archivos multimedia web|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|

## <a name="server-roles"></a>Roles del servidor
|Roles de Windows Server disponibles|Servicios de rol|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|----------|---------------------------|  
|Servicios de certificados de Active Directory| |Sí|Sí|
|Active Directory Domain Services| |Sí|Sí|
|Servicios de federación de Active Directory (AD FS)| |Sí|Sí|
|AD Lightweight Directory Services| |Sí|Sí|
|AD Rights Management Services| |Sí|Sí|
|Certificación de estado del dispositivo| |Sí|Sí|
|Servidor DHCP| |Sí|Sí|
|Servidor DNS| |Sí|Sí|
|Servidor de fax| |Sí|Sí|
|Servicios de archivos y almacenamiento|Servidor de archivos|Sí|Sí|
|Servicios de archivos y almacenamiento|BranchCache para archivos de red|Sí|Sí|
|Servicios de archivos y almacenamiento|Desduplicación de datos|Sí|Sí|
|Servicios de archivos y almacenamiento|Espacios de nombres DFS|Sí|Sí|
|Servicios de archivos y almacenamiento|Replicación DFS|Sí|Sí|
|Servicios de archivos y almacenamiento|Administrador de recursos del servidor de archivos|Sí|Sí|
|Servicios de archivos y almacenamiento|Servicio del agente VSS del servidor de archivos|Sí|Sí|
|Servicios de archivos y almacenamiento|Servidor de destino iSCSI|Sí|Sí|
|Servicios de archivos y almacenamiento|Proveedor de almacenamiento de destino iSCSI|Sí|Sí|
|Servicios de archivos y almacenamiento|Servidor para NFS|Sí|Sí|
|Servicios de archivos y almacenamiento|Carpetas de trabajo|Sí|Sí|
|Servicios de archivos y almacenamiento|Servicios de almacenamiento|Sí|Sí|
|Servicio de protección de host| |Sí|Sí|
|Hyper-V| |Sí|Sí; incluye máquinas virtuales blindadas|
|MultiPoint Services| |Sí|Sí|
|Controladora de red| |No|Sí|
|Servicios de acceso y directivas de redes| |Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Servicios de impresión y documentos| |Sí|Sí|
|Acceso remoto| |Sí|Sí|
|Servicios de Escritorio remoto| |Sí|Sí|
|Volume Activation Services| |Sí|Sí|
|Servicios web (IIS)| |Sí|Sí|
|Servicios de implementación de Windows| |Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Experiencia con Windows Server Essentials| |Sí|Sí|
|Windows Server Update Services| |Sí|Sí|

## <a name="features"></a>Características

|Características de Windows Server instalables con Administrador del servidor (o PowerShell)|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|Sí|Sí|
|.NET Framework 4.6|Sí|Sí|
|Servicio de transferencia inteligente en segundo plano (BITS)|Sí|Sí|
|Cifrado de unidad BitLocker|Sí|Sí|
|Desbloqueo de BitLocker en red|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|BranchCache|Sí|Sí|
|Cliente para NFS|Sí|Sí|
|Contenedores|Sí (contenedores de Windows ilimitados; hasta 2 contenedores de Hyper-V)|Sí (todos los tipos de contenedor ilimitados)|
|Protocolo de puente del centro de datos|Sí|Sí|
|DirectPlay|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Almacenamiento mejorado|Sí|Sí|
|Clústeres de conmutación por error|Sí|Sí|
|Administración de directivas de grupo|Sí|Sí|
|Compatibilidad de Hyper-V con protección de host|No|Sí|
|Calidad de servicio de E/S|Sí|Sí|
|Núcleo de web hospedable de IIS|Sí|Sí|
|Cliente de impresión en Internet|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Servidor IPAM|Sí|Sí|
|Servicio de servidor iSNS|Sí|Sí|
|Monitor de puerto de LPR|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Extensión IIS Management OData|Sí|Sí|
|Media Foundation|Sí|Sí|
|Cola de mensajes|Sí|Sí|
|E/S de múltiples rutas|Sí|Sí|
|MultiPoint Connector|Sí|Sí|
|Equilibrio de carga de red|Sí|Sí|
|Protocolo de resolución de nombres de mismo nivel|Sí|Sí|
|Windows Audio Video Experience (qWAVE)|Sí|Sí|
|Kit de administración de Connection Manager de RAS|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Asistencia remota|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Compresión diferencial remota|Sí|Sí|
|RSAT|Sí|Sí|
|Proxy RPC sobre HTTP|Sí|Sí|
|Recopilación de eventos de configuración y arranque|Sí|Sí|
|Servicios simples de TCP/IP|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS|Instaladas|Instaladas|
|Límite de ancho de banda SMB|Sí|Sí|
|Servidor SMTP|Sí|Sí|
|Servicio SNMP|Sí|Sí|
|Equilibrador de carga de software|No|Sí|
|Réplica de almacenamiento|No|Sí|
|Cliente Telnet|Sí|Sí|
|Cliente TFTP|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Herramientas de blindaje de máquinas virtuales para la administración de tejidos|Sí|Sí|
|Redirector WebDAV|Sí|Sí|
|Plataforma de biometría de Windows|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Características de Windows Defender|Instaladas|Instaladas|
|Windows Identity Foundation 3.5|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Windows Internal Database|Sí|Sí|
|Windows PowerShell|Instaladas|Instaladas|
|Servicio de activación de procesos de Windows|Sí|Sí|
|Servicio de Windows Search|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Copias de seguridad de Windows Server|Sí|Sí|
|Herramientas de migración de Windows Server|Sí|Sí|
|Administración de almacenamiento basada en estándares de Windows|Sí|Sí|
|Windows TIFF IFilter|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|
|Extensión IIS de WinRM|Sí|Sí|
|Servidor WINS|Sí|Sí|
|Servicio WLAN|Sí|Sí|
|Compatibilidad con WoW64|Instaladas|Instaladas|
|Visor de XPS|Sí, si se instala como servidor con Experiencia de escritorio|Sí, si se instala como servidor con Experiencia de escritorio|

|Características que por lo general están disponibles|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|Analizador de procedimientos recomendados|Sí|Sí|
|Direct Access|Sí|Sí|
|Memoria dinámica (en virtualización)|Sí|Sí|
|RAM de agregado o reemplazo en caliente|Sí|Sí|
|Microsoft Management Console|Sí|Sí|
|Interfaz de servidor básica|Sí|Sí|
|Equilibrio de carga de red|Sí|Sí|
|Windows PowerShell|Sí|Sí|
|Opción de instalación Server Core|Sí|Sí|
|Opción de instalación Nano Server|Sí|Sí|
|Administrador de servidores|Sí|Sí|
|SMB directo y SMB sobre RDMA|Sí|Sí|
|Redes definidas por software|No|Sí|
|Servicio de administración de almacenamiento|Sí|Sí|
|Espacios de almacenamiento|Sí|Sí|
|Espacios de almacenamiento directo|No|Sí|
|Volume Activation Services|Sí|Sí|
|Integración de VSS (Servicio de instantáneas de volumen)|Sí|Sí|
|Windows Server Update Services|Sí|Sí|
|Administrador de recursos del sistema de Windows|Sí|Sí|
|Registro de licencias del servidor|Sí|Sí|
|Activación heredada|Como invitado si se hospeda en el centro de datos|Puede ser host o invitado|
|Carpetas de trabajo|Sí|Sí|

