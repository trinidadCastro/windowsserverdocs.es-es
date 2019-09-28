---
title: Implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0dce886555167ad651704045120fb92eff0dcea1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356178"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar esta guía para implementar certificados de servidor en los servidores de infraestructura de acceso remoto y servidor de directivas de redes (NPS).   

En esta guía se incluyen las siguientes secciones.  

-   [Requisitos previos para usar esta guía](#bkmk_pre)  

-   [Qué no incluye esta guía](#bkmk_not)  

-   [Información general sobre tecnología](#bkmk_tech)  

-   [Introducción a la implementación de certificados de servidor](Server-Certificate-Deployment-Overview.md)  

-   [Planeación de la implementación de certificados de servidor](Server-Certificate-Deployment-Planning.md)  

-   [Implementación de certificados de servidor](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**Certificados de servidor digital**  
En esta guía se proporcionan instrucciones para usar Active Directory servicios de Certificate Server (AD CS) para inscribir automáticamente certificados en servidores de infraestructura NPS y de acceso remoto. AD CS le permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, certificados digitales y capacidades de firma digital para su organización.  

Cuando se usan certificados de servidor digital para la autenticación entre equipos de la red, los certificados proporcionan:   

1. Confidencialidad mediante cifrado.  
2. Integridad mediante firmas digitales.  
3. Autenticación mediante la Asociación de claves de certificado con cuentas de equipo, usuario o dispositivo en una red de equipos.  

### <a name="server-types"></a>**Tipos de servidor**  
Con esta guía, puede implementar certificados de servidor en los siguientes tipos de servidores.  
- Los servidores que ejecutan el servicio de acceso remoto, que son servidores de DirectAccess o de red privada virtual (VPN) estándar, y que son miembros del grupo de **servidores RAS e IAS** .  
- Servidores que ejecutan el servicio del servidor de directivas de redes (NPS) y que son miembros del grupo de **servidores RAS e IAS** .  

### <a name="advantages-of-certificate-autoenrollment"></a>**Ventajas de la inscripción automática de certificados**  
La inscripción automática de certificados de servidor, también denominada inscripción automática, proporciona las siguientes ventajas.  

- La entidad de certificación (CA) de AD CS inscribe automáticamente un certificado de servidor en todos los servidores de acceso remoto y NPS.  
- Todos los equipos del dominio reciben automáticamente el certificado de CA, que se instala en el almacén de entidades de certificación raíz de confianza en cada equipo miembro del dominio. Por este motivo, todos los equipos del dominio confían en los certificados emitidos por la CA. Esta confianza permite que los servidores de autenticación demuestren sus identidades entre sí y participen en comunicaciones seguras.  
- Aparte de actualizar directiva de grupo, no se requiere la reconfiguración manual de cada servidor.  
- Cada certificado de servidor incluye el propósito de autenticación del servidor y el propósito de autenticación del cliente en las extensiones de uso mejorado de clave (EKU).  
- Escalabilidad. Después de implementar la CA raíz de la empresa con esta guía, puede expandir la infraestructura de clave pública (PKI) agregando las CA subordinadas de la empresa.  
- Capacidad de administración. Puede administrar AD CS mediante la consola de AD CS o mediante comandos y scripts de Windows PowerShell.  
- Simplicidad. Los servidores que inscriben certificados de servidor se especifican mediante Active Directory cuentas de grupo y la pertenencia a grupos.   
- Al implementar certificados de servidor, los certificados se basan en una plantilla que se configura con las instrucciones de esta guía. Esto significa que puede personalizar distintas plantillas de certificado para tipos de servidor específicos, o puede usar la misma plantilla para todos los certificados de servidor que desee emitir.  

## <a name="bkmk_pre"></a>Requisitos previos para usar esta guía  

En esta guía se proporcionan instrucciones sobre cómo implementar certificados de servidor mediante AD CS y el rol de servidor servidor Web (IIS) en Windows Server 2016. A continuación se indican los requisitos previos para realizar los procedimientos de esta guía.  

- Debe implementar una red principal mediante la guía de red principal de Windows Server 2016, o bien debe tener las tecnologías proporcionadas en la guía de red principal instalada y funcionando correctamente en la red. Estas tecnologías incluyen TCP/IP V4, DHCP, Active Directory Domain Services (AD DS), DNS y NPS.  
  >[!NOTE]
  >La guía de red principal de Windows Server 2016 está disponible en la biblioteca técnica de Windows Server 2016. Para obtener más información, consulte [Guía de red principal](../../../core-network-guide/Core-Network-Guide.md).

- Debe leer la sección de planeación de esta guía para asegurarse de que está preparado para esta implementación antes de realizar la implementación.  
- Debe realizar los pasos de esta guía en el orden en que se presentan. No avance e implemente la CA sin realizar los pasos que conducen a la implementación del servidor, o bien se producirá un error en la implementación.  
- Debe estar preparado para implementar dos nuevos servidores en la red: un servidor en el que se instalará AD CS como una CA raíz de empresa y un servidor en el que se instalará el servidor Web (IIS) para que la CA pueda publicar la lista de revocación de certificados (CRL) en la web se rVer.   

>[!NOTE]  
>Está preparado para asignar una dirección IP estática a los servidores web y AD CS que implemente con esta guía, así como para asignar nombres a los equipos según las convenciones de nomenclatura de su organización. Además, debe unir los equipos al dominio.  

## <a name="bkmk_not"></a>Qué no proporciona esta guía  
En esta guía no se proporcionan instrucciones completas para diseñar e implementar una infraestructura de clave pública (PKI) mediante AD CS. Se recomienda revisar la documentación de AD CS y la documentación de diseño de PKI antes de implementar las tecnologías de esta guía.   

## <a name="bkmk_tech"></a>Información general sobre tecnología  
A continuación se muestran información general sobre la tecnología de AD CS y el servidor Web (IIS).  

### <a name="active-directory-certificate-services"></a>Servicios de certificados de Active Directory  
AD CS en Windows Server 2016 proporciona servicios personalizables para crear y administrar los certificados X. 509 que se usan en sistemas de seguridad de software que emplean tecnologías de clave pública. Las organizaciones pueden usar AD CS para mejorar la seguridad mediante el enlace de la identidad de una persona, un dispositivo o un servicio con la clave pública correspondiente. AD CS también incluye características que permiten administrar la inscripción y revocación de certificados en una variedad de entornos escalables.  

Para obtener más información, vea [información general de servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx) y guía de diseño de infraestructura de [clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Servidor web (IIS)  

El rol de servidor Web (IIS) en Windows Server 2016 proporciona una plataforma segura, fácil de administrar, modular y extensible para hospedar sitios web, servicios y aplicaciones de forma confiable. Con IIS, puede compartir información con usuarios en Internet, en una intranet o en una extranet. IIS es una plataforma web unificada que integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).  

Para obtener más información, vea [Introducción al servidor Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
