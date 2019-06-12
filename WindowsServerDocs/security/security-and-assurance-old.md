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
ms.openlocfilehash: fed0587b74873005f14a216bac22f952bcc65a4f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447297"
---
# <a name="security-and-assurance-in-windows-server"></a>Seguridad y control en Windows Server 

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<img src="../media/landing-icons/security.png" style='float:left; padding:.5em;' alt="Icon representing a lock"> Puedes confiar en las nuevas capas de protección integradas en el sistema operativo para una mayor protección contra las infracciones de seguridad. Ayuda a bloquear ataques malintencionados y a mejorar la seguridad de sus máquinas virtuales, aplicaciones y datos.


### <a name="windows-server-security-blog-posthttpsblogstechnetmicrosoftcomwindowsserver20160425ten-reasons-youll-love-windows-server-2016-8-security"></a>[Entrada de Blog de seguridad de Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/04/25/ten-reasons-youll-love-windows-server-2016-8-security/)
En esta entrada de blog del equipo de seguridad de Windows Server se resaltan muchas de las mejoras de Windows Server que aumentan la seguridad del hospedaje y de los entornos de nube híbridos.

### <a name="datacenter-and-private-cloud-security-bloghttpsblogstechnetmicrosoftcomdatacentersecurity"></a>[Blog de seguridad de la nube privada y centro de datos](https://blogs.technet.microsoft.com/datacentersecurity/)
Este es el sitio central del blog de contenido técnico del equipo de seguridad de centro de datos y nube privada de Microsoft.                                    

### <a name="addressing-emerging-threats-and-landscape-shiftshttpswwwyoutubecomwatchvb5jmyxywx1kfeatureyoutube"></a>[Desplaza abordar las amenazas emergentes y horizontal](https://www.youtube.com/watch?v=B5JMYxYWx1k&feature=youtu.be)
En este vídeo de 6 minutos, Anders Vinberg proporciona una visión general de la estrategia de seguridad y control de Microsoft y comenta las tendencias del sector y los cambios horizontales relacionados con la seguridad. Después se centra en iniciativas claves de Microsoft para proteger las cargas de trabajo desde el tejido subyacente y proteger también frente a ataques directos desde cuentas con privilegios. Por último, en caso de infracción, explica cómo las nuevas capacidades de detección y forenses pueden facilitar la identificación de la amenaza.

### <a name="protecting-your-datacenter-and-cloud-from-emerging-threats-blog-posthttpblogstechnetcombwindowsserverarchive20151118protecting-your-datacenter-and-cloud-november-updateaspx"></a>[Protección del centro de datos y en la nube de la entrada de blog Emerging Threats](http://blogs.technet.com/b/windowsserver/archive/2015/11/18/protecting-your-datacenter-and-cloud-november-update.aspx)
En esta entrada de blog se describe cómo puede usar tecnologías de Microsoft para proteger las inversiones del centro de datos y la nube frente a las amenazas emergentes.                   

### <a name="security-and-assurance-overview-session-at-ignitehttpchannel9msdncomeventsignite2015brk2482"></a>[Sesión de seguridad y la visión general del control en Ignite](http://channel9.msdn.com/events/ignite/2015/brk2482)
En esta sesión de Ignite se abordan las amenazas persistentes, las infracciones de información privilegiada, el cibercrímen organizado y la protección de la plataforma de nube de Microsoft (servicios locales y conectados con Azure). Incluye escenarios para proteger cargas de trabajo, inquilinos de grandes empresas y proveedores de servicios.                                                                   

## <a name="secure-virtualization-with-shielded-vms"></a>Virtualización segura con máquinas virtuales blindada

### <a name="shielded-vm-in-channel-9httpchannel9msdncomshowsmechanicsintroduction-to-shielded-virtual-machines-in-windows-server-2016"></a>[VM blindada en Channel 9](http://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
Tutorial de la tecnología de VM blindada y sus ventajas.                           

### <a name="shielded-vm-demohttpswwwyoutubecomwatchvxip5qtk-7d8"></a>[Demostración de la máquina virtual blindada](https://www.youtube.com/watch?v=xip5Qtk-7d8)
En este vídeo de 4 minutos se describe el valor de las máquinas virtuales blindadas y las diferencias entre una máquina virtual blindada y una no blindada.                                   

### <a name="shielded-virtual-machines-in-windows-server-video-walkthroughhttpmicrosoft-cloudcloudguidescomguidesshielded-virtual-machines-in-windows-serverhtm"></a>[Máquinas virtuales blindadas en el tutorial de vídeo de Windows Server](http://microsoft-cloud.cloudguides.com/Guides/Shielded Virtual Machines in Windows Server.htm)
En este tutorial de vídeo se muestra cómo el Servicio de protección de host habilita máquinas virtuales blindadas para que los datos confidenciales estén protegidos del acceso no autorizado por administradores del host de Hyper-V.

### <a name="harden-the-fabric-protecting-tenant-secrets-in-hyper-v-ignite-videohttpchannel9msdncomeventsignite2015brk3457"></a>[Protección del tejido: Proteger los secretos de inquilino en Hyper-V (vídeo de Ignite)](http://channel9.msdn.com/events/ignite/2015/brk3457)

En esta presentación de Ignite se describen las mejoras en Hyper-V, Virtual Machine Manager y un nuevo rol de servidor de protección de host para habilitar VM blindadas.                

### <a name="guarded-fabric-deployment-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-deploying-hgs-overview"></a>[Guía de implementación de tejido protegido](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)
En esta guía se proporciona información sobre la instalación y la validación de Windows Server y System Center Virtual Machine Manager para los host de tejido protegido y las VM blindadas.

### <a name="shielded-vm-and-guarded-fabric-in-branch-officeshttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-manage-branch-office"></a>[Máquinas virtuales blindadas y tejido protegido en las sucursales](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office)
Esta guía proporciona procedimientos recomendados para ejecutar máquinas virtuales blindadas en sucursales y otros escenarios remotos, donde los hosts de Hyper-V pueden tener periodos de tiempo con conectividad limitada a HGS.

### <a name="shielded-vm-and-guarded-fabric-troubleshooting-guidehttpsdocsmicrosoftcomwindows-servervirtualizationguarded-fabric-shielded-vmguarded-fabric-troubleshoot-overview"></a>[Máquinas virtuales blindadas y tejido protegido Guía de solución de problemas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-overview)
En esta guía se proporciona información sobre cómo resolver los problemas que puede encontrar en el entorno de máquinas virtuales blindadas.

### <a name="shielded-vm-articlehttpwindowsitprocomhyper-vsuper-secure-hyper-v-environments-shielded-vms-2016"></a>[Artículo de la máquina virtual blindada](http://windowsitpro.com/hyper-v/super-secure-hyper-v-environments-shielded-vms-2016)
En este artículo se proporciona información general sobre cómo las máquinas virtuales blindadas proporcionan una mayor seguridad para impedir la manipulación.                                         

## <a name="privileged-access-management"></a>Privileged Access Management
### <a name="securing-privileged-accesshttpstechnetmicrosoftcomwindows-server-docssecuritysecuring-privileged-accesssecuring-privileged-access"></a>[Protección del acceso con privilegios](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access)
Un mapa de ruta de cómo proteger el acceso con privilegios. Este mapa de ruta se genera en función de la experiencia combinada del equipo de seguridad de servidor, Microsoft IT, el equipo de Azure y los Servicios de consultoría de Microsoft                           

### <a name="just-in-time-administration-with-microsoft-identity-managerhttpstechnetmicrosoftcomlibrarymt150258aspx"></a>[Just-in-administración de Time con Microsoft Identity Manager](https://technet.microsoft.com/library/mt150258.aspx)
En este artículo se describen las características y funcionalidades incluidas en Microsoft Identity Manager, como la compatibilidad con Privileged Access Management Just In Time (JIT).                                                                    

### <a name="protecting-windows-and-microsoft-azure-active-directory-with-privileged-access-managementhttpchannel9msdncomeventsignite2015brk3873"></a>[Protección de Windows y Microsoft Azure Active Directory con Privileged Access Management](http://channel9.msdn.com/events/ignite/2015/brk3873)
En esta presentación de Ignite se tratan la estrategia de Microsoft y las inversiones en Windows Server, PowerShell, Active Directory, Identity Manager y Azure Active Directory para abordar los riesgos del acceso de administrador mediante autenticación segura y la administración del acceso mediante Just-in-Time y Just Enough Administration (JEA).

### <a name="just-enough-administration-articlehttpakamsjea"></a>[Artículo de Just Enough Administration](http://aka.ms/JEA)
En este documento se comparten la versión y los detalles técnicos de Just Enough Administration, un conjunto de herramientas de PowerShell diseñado para ayudar a las organizaciones a reducir el riesgo al restringir a los operadores el acceso a solo el necesario para realizar tareas específicas.

### <a name="just-enough-administration-demo-videohttpswwwyoutubecomwatchvxnbrbky9p20"></a>[Solo el vídeo de demostración Enough Administration](https://www.youtube.com/watch?v=xnBrbkY9P20)
Tutorial de demostración de Just Enough Administration.                                                                                                                  
## <a name="credential-protection"></a>Protección de credenciales

### <a name="protect-derived-domain-credentials-with-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectioncredential-guardcredential-guard"></a>[Proteger las credenciales de dominio derivadas con Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/credential-guard/credential-guard)
Credential Guard usa la seguridad basada en virtualización para aislar los secretos de forma que solo el software de sistema con privilegios pueda acceder a ellos. El acceso no autorizado a estos secretos puede derivar en ataques de robo de credenciales, como ataques pass-the-hash o pass-the-ticket. Credential Guard impide estos ataques mediante la protección de los hash de contraseña NTLM y los vales de concesión de vales Kerberos.

### <a name="protect-remote-desktop-credentials-with-remote-credential-guardhttpsdocsmicrosoftcomwindowssecurityidentity-protectionremote-credential-guard"></a>[Proteger las credenciales de Escritorio remoto con Credential Guard remoto](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard)
Credential Guard remoto ayuda a proteger las credenciales a través de una conexión a Escritorio remoto al redirigir las solicitudes de Kerberos de vuelta al dispositivo que solicita la conexión. También proporciona experiencias de inicio de sesión único para sesiones de Escritorio remoto.                                                                                                        |
### <a name="credential-guard-demo-videohttpswwwyoutubecomwatchveupkogsl7yk"></a>[Vídeo de demostración de Credential Guard](https://www.youtube.com/watch?v=eUpKOGSl7yk)
En este vídeo de 5 minutos se presentan demostraciones de vídeo de Credential Guard y Credential Guard remoto.         

## <a name="hardening-the-os-and-applications"></a>Protección del sistema operativo y las aplicaciones
### <a name="windows-defender-application-control-wdac-deployment-guidehttpsdocsmicrosoftcomwindowssecuritythreat-protectionwindows-defender-application-controlwindows-defender-application-control"></a>[Guía de implementación de Windows Defender Application Control (WDAC)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)
WDAC es una directiva configurable de integridad de código (CI) que ayuda a las empresas a controlar qué aplicaciones se ejecutan en sus entornos y que no conlleva otros requisitos específicos de hardware o software que no sean ejecutar Windows 10.

### <a name="device-guard-demo-videohttpswwwyoutubecomwatchvf-ptkesjkhi"></a>[Vídeo de demostración de protección de dispositivos](https://www.youtube.com/watch?v=F-pTkesjkhI)
Device Guard es una combinación de WDAC y de integridad de código protegida por hipervisor (HVCI). Este vídeo de 7 minutos presenta Device Guard y su uso en Windows Server.

### <a name="transport-layer-security-registry-settingshttpsdocsmicrosoftcomwindows-serversecuritytlstls-registry-settings"></a>[Configuración de registro de seguridad de capa de transporte](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)
Información de configuración de Registro soportada para la implementación en Windows del protocolo de Seguridad de la capa de transporte (TLS) y el protocolo Capa de sockets seguros (SSL).

### <a name="control-flow-guardhttpsdocsmicrosoftcomwindowsdesktopsecbpcontrol-flow-guard"></a>[Protección de flujo de control](https://docs.microsoft.com/windows/desktop/SecBP/control-flow-guard)
Protección de flujo de control proporciona protección integrada frente a algunas clases de ataques de corrupción de memoria.

### <a name="windows-defenderhttpstechnetmicrosoftcomwindows-server-docssecuritywindows-defenderwindows-defender-overview-windows-server"></a>[Windows Defender](https://technet.microsoft.com/windows-server-docs/security/windows-defender/windows-defender-overview-windows-server)
Windows Defender proporciona capacidades de detección activas para bloquear malware conocido. Windows Defender está activado de forma predeterminada y está optimizado para admitir los distintos roles de servidor en Windows Server.

## <a name="detecting-and-responding-to-threats"></a>Detección y respuesta a las amenazas
### <a name="security-threat-analysis-using-microsoft-operations-management-suitehttpschannel9msdncomeventsignite2015brk3464"></a>[Análisis de amenazas de seguridad con Microsoft Operations Management Suite](https://channel9.msdn.com/events/ignite/2015/brk3464)
En esta presentación de Ignite se describe cómo puede usar Operational Insights para realizar el análisis de las amenazas de seguridad.

### <a name="microsoft-operations-management-suite-omshttpswwwmicrosoftcomen-usserver-cloudoperations-management-suiteoverviewaspx"></a>[Microsoft Operations Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx)
La solución de seguridad y auditoría de Microsoft Operations Management Suite (OMS) procesa los registros de seguridad y los eventos de firewall de entornos locales y de nube para analizar y detectar comportamientos malintencionados.

### <a name="oms-and-windows-serverhttpswwwyoutubecomwatchvsadw1dry2k"></a>[OMS y Windows Server](https://www.youtube.com/watch?v=_SaDw1dRy2k)
En este vídeo de 3 minutos se muestra cómo OMS puede ayudar a detectar posibles comportamientos malintencionados bloqueados por Windows Server.  

### <a name="microsoft-advanced-threat-analyticshttpblogstechnetcombadarchive20150722microsoft-advanced-threat-analytics-coming-next-monthaspx"></a>[Microsoft Advanced Threat Analytics](http://blogs.technet.com/b/ad/archive/2015/07/22/microsoft-advanced-threat-analytics-coming-next-month.aspx)
En esta entrada de blog se analiza Microsoft Advanced Threat Analytics, un producto local que usa el tráfico de red de Active Directory y los datos de SIEM para detectar posibles amenazas y alertar sobre ellas.

### <a name="microsoft-advanced-threat-analyticshttpswwwyoutubecomwatchv0na9fetrzfwlistpl8nfc9hageb5izgm8hvmrozethrpbdksw"></a>[Microsoft Advanced Threat Analytics](https://www.youtube.com/watch?v=0nA9FeTRZFw&list=PL8nfc9haGeb5IZGM8HvmRozetHRpBDKSw)
En este vídeo de 3 minutos se presenta una visión general sobre cómo Microsoft está agregando funcionalidades de análisis de amenazas en Windows Server.                                                                                 |

## <a name="network-security"></a>Seguridad de red

### <a name="datacenter-firewall-overviewhttpstechnetmicrosoftcomlibrarydn920240aspx"></a>[Información general de Datacenter Firewall](https://technet.microsoft.com/library/dn920240.aspx)
Aquí se describe Datacenter Firewall, un firewall multinquilino, con estado, de nivel de red y 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino).

### <a name="whats-new-in-dns-in-windows-serverhttpstechnetmicrosoftcomwindows-server-docsnetworkingdnswhat-s-new-in-dns-server"></a>[Novedades de DNS en Windows Server](https://technet.microsoft.com/windows-server-docs/networking/dns/what-s-new-in-dns-server)
En este tema se proporcionan breves descripciones de nuevas funcionalidades de DNS, junto con vínculos a más información.                                                                           

## <a name="mapping-security-features-to-compliance-regulations"></a>Asignación de características de seguridad a los reglamentos de cumplimiento

El cumplimiento es un aspecto importante de las características de seguridad. Se proporciona asesoramiento profesional sobre cómo lograr el cumplimiento y se explica en qué se asimila el cumplimiento a los asesores de cumplimiento de confianza, pero también se pretende proporcionar una asignación inicial para poder utilizarla al evaluar Windows Server.

-   [Notas del producto sobre asignación de cumplimiento de máquinas virtuales blindadas de Hyper-V](https://download.microsoft.com/download/6/D/0/6D06E149-B4C1-4EED-ACD5-DF6066E93CC0/Coalfire_Branded_Hyper_V_Shielded_VMs_Whitepaper_EN_US.pdf)

-   [Documento de asignación de cumplimiento de JEA y JIT](https://download.microsoft.com/download/2/7/A/27A2B5BB-6B52-4482-87C1-DA9D6B6D8C8D/Coalfire_Branded_Privileged_Identity_Manager_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre asignación de cumplimiento de dispositivo Guard](https://download.microsoft.com/download/6/9/D/69D9E610-D23C-4F7E-A8CC-D65B87CEB4F8/Coalfire_Branded_Device_Guard_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre asignación de cumplimiento de Credential Guard](https://download.microsoft.com/download/8/1/2/812009C9-E4B8-4D4B-AADD-FDC373D0A076/Coalfire_Branded_Credential_Guard_Whitepaper_EN_US.pdf)

-   [Notas del producto sobre asignación de cumplimiento de Windows Defender](https://download.microsoft.com/download/C/7/7/C778B7BB-0783-42D7-93A9-B86DFB5A7BAD/Coalfire_Branded_Windows_Defender_Whitepaper_EN_US.pdf)
