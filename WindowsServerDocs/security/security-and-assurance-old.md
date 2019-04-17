---
title: Seguridad y control
description: Información general sobre seguridad en Windows Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/27/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: b32b4879ad454d1154c3d65dbf690cdaae73d76c
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121454"
---
# Seguridad y control en Windows Server 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> Puedes confiar en las nuevas capas de protección integradas en el sistema operativo para una mayor protección contra las infracciones de seguridad. Ayudan a bloquear ataques malintencionados y a mejorar la seguridad de tus máquinas virtuales, aplicaciones y datos.


### [Entrada de blog de seguridad de Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
En esta entrada de blog del equipo de seguridad de Windows Server se resaltan muchas de las mejoras de Windows Server que aumentan la seguridad del hospedaje y de los entornos de nube híbridos.

### [Blog de seguridad de centro de datos y nube privada](https://blogs.technet.microsoft.com/datacentersecurity/)
Este es el sitio central del blog de contenido técnico del equipo de seguridad del centro de datos y la nube privada de Microsoft.                                    

### [Cómo abordar las amenazas emergentes y los cambios en el panorama de la seguridad](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
En este vídeo de 6 minutos, Anders Vinberg proporciona una visión general de la estrategia de seguridad y control de Microsoft y comenta las tendencias del sector y los cambios en el panorama de la seguridad, en la medida en que la afectan. Después se centra en iniciativas claves de Microsoft para proteger las cargas de trabajo desde el tejido subyacente y proteger también frente a ataques directos desde cuentas con privilegios. Por último, en caso de infracción, explica cómo las nuevas capacidades de detección y forenses pueden facilitar la identificación de la amenaza.

### [Entrada del blog de protección del centro de datos y la nube de amenazas emergentes](http://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
En esta entrada de blog se describe cómo puedes usar tecnologías de Microsoft para proteger las inversiones del centro de datos y la nube frente a las amenazas emergentes.                   

### [Sesión de introducción sobre seguridad y control en Ignite](http://channel9.msdn.com/events/ignite/2015/brk2482)
En esta sesión de Ignite se abordan las amenazas persistentes, las infracciones desde el interior, el cibercrimen organizado y la protección de la plataforma Microsoft Cloud (servicios locales y conectados con Azure). Incluye escenarios para proteger cargas de trabajo, inquilinos de grandes empresas y proveedores de servicios.                                                                   

## Virtualización segura con VM blindadas

### [VM blindada en Channel 9](http://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
Tutorial de la tecnología de VM blindada y sus ventajas.                           

### [Demostración de VM blindada](https://www.youtube.com/watch?v=xip5Qtk-7d8)
En este vídeo de 4 minutos se describe el valor de las VM blindadas y las diferencias entre una VM blindada y una no blindada.                                   

### [Tutorial de vídeo sobre máquinas virtuales blindadas en Windows Server](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
En este tutorial de vídeo se muestra cómo el Servicio de protección de host habilita máquinas virtuales blindadas para que los datos confidenciales estén protegidos del acceso no autorizado por administradores del host de Hyper-V.

### [Protección del tejido: protección de los secretos de inquilinos en Hyper-V (vídeo de Ignite)](http://channel9.msdn.com/events/ignite/2015/brk3457)

En esta presentación de Ignite se describen las mejoras en Hyper-V, Virtual Machine Manager y un nuevo rol de servidor de protección de host para habilitar VM blindadas.                

### [Guía de implementación del tejido protegido](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
En esta guía se proporciona información sobre la instalación y la validación de Windows Server y System Center Virtual Machine Manager para los host de tejido protegido y las VM blindadas.

### [VM blindadas y Tejido protegido en sucursales](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
Esta guía proporciona procedimientos recomendados para ejecutar máquinas virtuales blindadas en sucursales y otros escenarios remotos, donde los hosts de Hyper-V pueden tener periodos de tiempo con conectividad limitada a HGS.

### [Guía de solución de problemas de VM blindadas y tejido protegido](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
En esta guía se proporciona información sobre cómo resolver los problemas que puedes encontrar en el entorno de VM blindadas.

### [Artículo sobre VM blindadas](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
En estas notas del producto se proporciona información general sobre cómo las VM blindadas proporcionan una mayor seguridad general para impedir la manipulación.                                         

## Privileged Access Management
### [Protección del acceso con privilegios](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
Un mapa de ruta de cómo proteger el acceso con privilegios. Este mapa de ruta se genera en función de la experiencia combinada del equipo de seguridad de servidores, TI de Microsoft, el equipo de Azure y los Servicios de consultoría de Microsoft                           

### [Administración Just-in-Time con Microsoft Identity Manager](https://technet.microsoft.com/library/mt150258.aspx)
En este artículo se describen las funciones y funcionalidades incluidas en Microsoft Identity Manager, incluyendo la compatibilidad con Privileged Access Management Just In Time (JIT).                                                                    

### [Protección de Windows y Microsoft Azure Active Directory con Privileged Access Management](http://channel9.msdn.com/events/ignite/2015/brk3873)
En esta presentación de Ignite se tratan la estrategia de Microsoft y las inversiones en Windows Server, PowerShell, Active Directory, Identity Manager y Azure Active Directory para abordar los riesgos del acceso de administrador mediante autenticación segura y la administración del acceso mediante Just-in-Time y Just Enough Administration (JEA).

### [Artículo sobre Just Enough Administration](http://aka.ms/JEA)
En este documento se comparten la visión y los detalles técnicos de Just Enough Administration, un conjunto de herramientas de PowerShell diseñado para ayudar a las organizaciones a reducir el riesgo al restringir a los operadores el acceso a solo el necesario para realizar tareas específicas.

### [Vídeo de demostración de Just Enough Administration](https://www.youtube.com/watch?v=xnBrbkY9P20)
Tutorial de demostración de Just Enough Administration.                                                                                                                  
## Protección de credenciales

### [Proteger las credenciales de dominio derivadas con Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
Credential Guard usa la seguridad basada en virtualización para aislar los secretos, de forma que solo el software de sistema con privilegios pueda acceder a ellos. El acceso no autorizado a estos secretos puede derivar en ataques de robo de credenciales, como ataques pass-the-hash o pass-the-ticket. Credential Guard impide estos ataques mediante la protección de los hash de contraseña NTLM y los tiques de concesión de tiques Kerberos.

### [Proteger las credenciales de Escritorio remoto con Credential Guard remoto](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
Credential Guard remoto ayuda a proteger las credenciales a través de una conexión a Escritorio remoto al redirigir las solicitudes de Kerberos de vuelta al dispositivo que solicita la conexión. También proporciona experiencias de inicio de sesión único para sesiones de Escritorio remoto.                                                                                                        |
### [Vídeo de demostración de Credential Guard](https://www.youtube.com/watch?v=eUpKOGSl7yk)
En este vídeo de 5 minutos se presentan demostraciones de vídeo de Credential Guard y Credential Guard remoto.         

## Protección del sistema operativo y las aplicaciones
### [Guía de desarrollo de Control de aplicaciones de Windows Defender (WDAC)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
WDAC es una directiva configurable de integridad de código (CI) que ayuda a las empresas a controlar qué aplicaciones se ejecutan en sus entornos y que no conlleva otros requisitos específicos de hardware o software que no sean ejecutar Windows 10.

### [Vídeo de demostración de Device Guard](https://www.youtube.com/watch?v=F-pTkesjkhI)
Device Guard es una combinación de WDAC y de integridad de código protegida por hipervisor (HVCI). Este vídeo de 7 minutos presenta Device Guard y su uso en Windows Server.

### [Configuración del Registro de Seguridad de la capa de transporte](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
Información de configuración de Registro soportada para la implementación en Windows del protocolo de Seguridad de la capa de transporte (TLS) y el protocolo Capa de sockets seguros (SSL).

### [Protección de flujo de control](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
Protección de flujo de control ofrece protección integrada frente a algunas clases de ataques de corrupción de memoria.

### [Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
Windows Defender proporciona capacidades de detección activa para bloquear malware conocido. Windows Defender está activado de forma predeterminada y está optimizado para admitir los distintos roles de servidor en Windows Server.

##Detección y respuesta a las amenazas
### [Análisis de las amenazas de seguridad mediante Microsoft Operations Management Suite](https://channel9.msdn.com/events/ignite/2015/brk3464)
En esta presentación de Ignite se describe cómo puedes usar Operational Insights para realizar el análisis de las amenazas de seguridad.

### [Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx)
La solución de seguridad y auditoría Microsoft Operations Management Suite (OMS) procesa los registros de seguridad y los eventos de firewall de entornos locales y de nube para analizar y detectar comportamientos malintencionados.

### [OMS y Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
En este vídeo de 3 minutos se muestra cómo OMS puede ayudar a detectar posibles comportamientos malintencionados bloqueados por Windows Server.  

### [Microsoft Advanced Threat Analytics](http://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
En esta entrada de blog se analiza Microsoft Advanced Threat Analytics, un producto local que usa el tráfico de red de Active Directory y los datos de SIEM para detectar posibles amenazas y alertar sobre ellas.

### [Microsoft Advanced Threat Analytics](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
En este vídeo de 3 minutos se presenta una visión general sobre cómo Microsoft está agregando funcionalidades de análisis de amenazas en Windows Server.                                                                                 |

## Seguridad de red

### [Información general de Datacenter Firewall](https://technet.microsoft.com/library/dn920240.aspx)
Esta información general trata sobre Datacenter Firewall, una capa de red, 5 tuplas (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino), el estado y un firewall de varios inquilinos.

### [Novedades de DNS en Windows Server](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
En este tema se proporcionan breves descripciones de nuevas funcionalidades de DNS, junto con vínculos a más información.                                                                           

## Asignación de funciones de seguridad a los reglamentos de cumplimiento

El cumplimiento es un aspecto importante de las funciones de seguridad. Se proporciona asesoramiento profesional sobre cómo lograr el cumplimiento y se explica en qué se asimila el cumplimiento a los asesores de cumplimiento de confianza, pero también se pretende proporcionar una asignación inicial para poder utilizarla al evaluar Windows Server.

-   [Notas del producto sobre la asignación de cumplimiento de VM blindadas de Hyper-V](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre la asignación de cumplimiento de JEA y JIT](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre la asignación de cumplimiento de Device Guard](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre la asignación de cumplimiento de Credential Guard](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre la asignación de cumplimiento de Windows Defender](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)
