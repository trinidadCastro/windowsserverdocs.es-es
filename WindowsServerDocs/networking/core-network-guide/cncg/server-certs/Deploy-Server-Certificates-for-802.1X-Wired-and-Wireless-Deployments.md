---
title: Implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d4cdcd11e0eb334064ddefec0eda775ffccff2c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446471"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar a esta guía para implementar certificados de servidor en los servidores de infraestructura de acceso remoto y servidor de directivas de redes (NPS).   

En esta guía se incluyen las siguientes secciones.  

-   [Requisitos previos para usar esta guía](#bkmk_pre)  

-   [Qué no incluye esta guía](#bkmk_not)  

-   [Introducción a las tecnologías](#bkmk_tech)  

-   [Información general de implementación de certificados de servidor](Server-Certificate-Deployment-Overview.md)  

-   [Planeación de la implementación de certificados de servidor](Server-Certificate-Deployment-Planning.md)  

-   [Implementación de certificados de servidor](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**Certificados de servidor digital**  
Esta guía proporciona instrucciones para usar los servicios de certificados de Active Directory (AD CS) para la inscripción automática de certificados para el acceso remoto y los servidores de infraestructura NPS. AD CS le permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, certificados digitales y las capacidades de firma digital de su organización.  

Cuando se usan certificados de servidor digital para la autenticación entre los equipos de la red, los certificados proporcionan:   

1. Confidencialidad mediante cifrado.  
2. Integridad mediante firmas digitales.  
3. Autenticación mediante la asociación de claves de certificado con cuentas de equipo, usuario o dispositivo en una red de equipo.  

### <a name="server-types"></a>**Tipos de servidor**  
Mediante el uso de esta guía, puede implementar certificados de servidor a los siguientes tipos de servidores.  
- Servidores que ejecutan el servicio de acceso remoto, que son DirectAccess o servidores de red privada virtual (VPN) estándar, y que son miembros de la **servidores RAS e IAS** grupo.  
- Los servidores que ejecutan el servicio servidor de directivas de redes (NPS) que son miembros de la **servidores RAS e IAS** grupo.  

### <a name="advantages-of-certificate-autoenrollment"></a>**Ventajas de la inscripción automática de certificados**  
La inscripción automática de certificados de servidor, que también se denomina la inscripción automática, proporciona las siguientes ventajas.  

- La entidad de certificación (CA) de AD CS inscribe automáticamente un certificado de servidor a todos los servidores NPS y acceso remoto.  
- Todos los equipos del dominio recibirán automáticamente el certificado de CA, que se instala en las entidades de certificación raíz de confianza almacena en cada equipo miembro del dominio. Por este motivo, todos los equipos del dominio confían en los certificados emitidos por una entidad de certificación. Esta relación de confianza permite que los servidores de autenticación para demostrar su identidad entre sí y participar en comunicaciones seguras.  
- Que no sea actualizar directiva de grupo, no se requiere la reconfiguración manual de todos los servidores.  
- Cada certificado de servidor incluye el propósito de autenticación del servidor y el propósito de autenticación de cliente en extensiones de uso mejorado de clave (EKU).  
- Escalabilidad. Después de implementar la entidad de certificación raíz de empresa con esta guía, puede expandir la infraestructura de clave pública (PKI) mediante la adición de CA subordinada de empresa.  
- Capacidad de administración. Puede administrar AD CS mediante la consola de AD CS o mediante el uso de scripts y comandos de Windows PowerShell.  
- Simplicidad. Especifique los servidores que inscriben certificados de servidor mediante el uso de las cuentas de grupo de Active Directory y pertenencia a grupos.   
- Al implementar los certificados de servidor, los certificados se basan en una plantilla que se configura con las instrucciones de esta guía. Esto significa que puede personalizar las plantillas de certificado diferente para cada tipo de servidor, o puede usar la misma plantilla para todos los certificados de servidor que desea emitir.  

## <a name="bkmk_pre"></a>Requisitos previos para usar esta guía  

Esta guía proporciona instrucciones sobre cómo implementar certificados de servidor mediante el uso de AD CS y el rol de servidor servidor Web (IIS) en Windows Server 2016. Estos son los requisitos previos para llevar a cabo los procedimientos de esta guía.  

- Debe implementar una red principal con la Guía de red de Windows Server 2016 Core, o ya debe tener las tecnologías proporcionadas en la Guía de red principal instalado y funciona correctamente en la red. Estas tecnologías incluyen TCP/IP v4, DHCP, Active Directory Domain Services (AD DS), DNS y NPS.  
  >[!NOTE]
  >La Guía de red de Windows Server 2016 Core está disponible en la biblioteca técnica de Windows Server 2016. Para obtener más información, consulte [Guía de red principal](../../../core-network-guide/Core-Network-Guide.md).

- Debe leer la sección de planeación de esta guía para asegurarse de que estén preparados para esta implementación antes de realizar la implementación.  
- Debe realizar los pasos de esta guía en el orden en que se presentan. No pasar la sección e implementar una entidad de certificación sin realizar los pasos que conducen a implementar el servidor o la implementación se producirá un error.  
- Debe estar preparado para implementar dos servidores nuevos de la red - un servidor en el que va a instalar AD CS como una CA raíz de empresa y un servidor en el que se instalará el servidor Web (IIS) para que la entidad de certificación puede publicar la lista de revocación de certificados (CRL) en la Web se ase.   

>[!NOTE]  
>Está preparado para asignar una dirección IP estática a los servidores Web y AD CS que implemente con esta guía, así como para el nombre de los equipos según las convenciones de nomenclatura de la organización. Además, debe unir los equipos al dominio.  

## <a name="bkmk_not"></a>¿Qué incluye esta guía no  
Esta guía no proporciona instrucciones detalladas para diseñar e implementar una infraestructura de clave pública (PKI) mediante el uso de AD CS. Se recomienda que revise la documentación de AD CS y PKI diseño antes de implementar las tecnologías de esta guía.   

## <a name="bkmk_tech"></a>Introducción a las tecnologías  
Estos son información general de tecnología para AD CS y el servidor Web (IIS).  

### <a name="active-directory-certificate-services"></a>Servicios de certificados de Active Directory  
AD CS en Windows Server 2016 ofrece servicios personalizables para crear y administrar los certificados X.509 que se usan en sistemas de seguridad de software que emplean tecnologías de clave pública. Las organizaciones pueden usar AD CS para mejorar la seguridad al enlazar la identidad de una persona, dispositivo o servicio con la clave pública correspondiente. AD CS también incluye características que permiten administrar la inscripción y revocación de una variedad de entornos escalables.  

Para obtener más información, consulte [Active Directory Certificate Services Overview](https://technet.microsoft.com/library/hh831740.aspx) y [Guía de diseño de infraestructura de clave pública](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Servidor web (IIS)  

El rol de servidor Web (IIS) en Windows Server 2016 proporciona una plataforma segura de administrar, modular y extensible para hospedar sitios Web, servicios y aplicaciones de forma confiable. Con IIS, puede compartir información con usuarios en Internet, una intranet o extranet. IIS es una plataforma web unificada que integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).  

Para obtener más información, consulte [información general del servidor Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
