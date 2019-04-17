---
title: Implementar certificados de servidor para 802.1X implementaciones inalámbricas y con cables
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fda9b2b53d088cc389e98cf96fecd748419bc6e2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Implementar certificados de servidor para 802.1X implementaciones inalámbricas y con cables

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar a esta guía para implementar certificados de servidor en los servidores de infraestructura de acceso remoto y el servidor de directivas de redes (NPS).   

Esta guía contiene las siguientes secciones.  

-   [Requisitos previos para usar esta guía](#bkmk_pre)  

-   [Lo que no proporciona esta guía](#bkmk_not)  

-   [Información general de la tecnología](#bkmk_tech)  

-   [Introducción de implementación de certificados de servidor](Server-Certificate-Deployment-Overview.md)  

-   [Planear la implementación de certificados de servidor](Server-Certificate-Deployment-Planning.md)  

-   [Implementación de certificados de servidor](Server-Certificate-Deployment.md)  

### **<a name="digital-server-certificates"></a>Certificados de servidor digital**  
Esta guía brinda instrucciones para usar los servicios de certificados de Active Directory (AD CS) para inscribir automáticamente los certificados para acceso remoto y servidores de la infraestructura NPS. AD CS te permite crear una infraestructura de clave pública (PKI) y proporcionar criptografía de clave pública, los certificados digitales y capacidades de firma digital para la organización.  

Al usar certificados de servidor digital para la autenticación entre equipos de la red, se proporcionan los certificados:   

1. Confidencialidad mediante cifrado.  
2. Integridad mediante las firmas digitales.  
3. Autenticación mediante la asociación de las claves de certificado con cuentas de equipo, el usuario o el dispositivo en una red del equipo.  

### **<a name="server-types"></a>Tipos de servidor**  
Mediante el uso de esta guía, puedes implementar los certificados de servidor en los siguientes tipos de servidores.  
- Servidores que ejecutan el servicio de acceso remoto, que son DirectAccess o servidores de red privada virtual (VPN) estándar, y que son miembros de la **RAS y servidores IAS** grupo.  
- Servidores que ejecutan el servicio de servidor de directivas de redes (NPS) que son miembros de la **RAS y servidores IAS** grupo.  

### **<a name="advantages-of-certificate-autoenrollment"></a>Ventajas de la inscripción automática de certificados**  
Inscripción automática de certificados de servidor, que también se denomina la inscripción automática, proporciona las siguientes ventajas.  

- La entidad de certificación (CA) de AD CS inscribe automáticamente un certificado de servidor para todos los servidores NPS y acceso remoto.  
- Todos los equipos del dominio reciben automáticamente el certificado de entidad emisora de certificados, que se instala en las entidades de certificación raíz de confianza de la tienda en cada equipo miembro del dominio. Por este motivo, todos los equipos del dominio confían en los certificados emitidos por la entidad de certificación. Esta confianza permite a los servidores de autenticación para que demuestran su identidad entre sí y participar en comunicaciones seguras.  
- Que no sean de actualizar la directiva de grupo, la reconfiguración manual de cada servidor no es necesaria.  
- Cada certificado de servidor incluye el propósito de autenticación del servidor y el propósito de autenticación de cliente en las extensiones de uso mejorado de claves (EKU).  
- Escalabilidad. Después de implementar la CA de raíz de empresa con esta guía, puedes expandir la infraestructura de clave pública (PKI) mediante la adición de CA subordinadas de empresa.  
- Facilidad de uso. Puedes administrar AD CS mediante la consola de AD CS o mediante el uso de scripts y comandos de Windows PowerShell.  
- Simplicidad. Especificar los servidores que inscripción certificados de servidor mediante el uso de cuentas de grupo de Active Directory y la pertenencia al grupo.   
- Cuando implementas certificados de servidor, los certificados se basan en una plantilla que se configura con las instrucciones que aparecen en esta guía. Esto significa que puedes personalizar las plantillas de certificado diferente para tipos de servidor específicos, o puedes usar la misma plantilla para todos los certificados de servidor que desea emitir.  

## <a name="bkmk_pre"></a>Requisitos previos para usar esta guía  

Esta guía brinda instrucciones sobre cómo implementar los certificados de servidor mediante AD CS y el rol de servidor Web Server (IIS) en Windows Server 2016. Los siguientes son los requisitos previos para llevar a cabo los procedimientos descritos en esta guía.  

- Debes implementar una red principal con la Guía de red principal de Windows Server 2016 o es necesario disponer de las tecnologías que se proporciona en la guía básica de red instalado y funcione correctamente en la red. Estas tecnologías incluyen TCP/IP v4, DHCP, Active Directory Domain Services (AD DS), DNS y NPS.  
>[!NOTE]
>La Guía de red de Windows Server 2016 Core está disponible en la biblioteca técnica de Windows Server 2016. Para obtener más información, consulta [guía básica de red](../../../core-network-guide/Core-Network-Guide.md).

- Se debe leer la sección de planeamiento de esta guía para asegurarse de que estén preparados para esta implementación antes de realizar la implementación.  
- Debes realizar los pasos de esta guía en el orden en que se presentan. No adelantarte e implementar la entidad de certificación sin realizar los pasos que conducen a implementar el servidor o la implementación se producirá un error.  
- Debe estar preparado para implementar dos nuevos servidores en su red, un servidor en el que vayas a instalar AD CS como una entidad de certificación raíz de la empresa y un servidor en el que vayas a instalar servidor Web (IIS) para que la entidad emisora de certificados puede publicar la lista de revocación de certificados (CRL) en el servidor Web.   

>[!NOTE]  
>Estás preparado para asignar una dirección IP estática a los servidores Web y AD CS que vas a implementar con esta guía, así como el nombre de los equipos según las convenciones de nomenclatura de la organización. Además, debes unir los equipos al dominio.  

## <a name="bkmk_not"></a>Lo que no proporciona esta guía  
Esta guía no proporciona instrucciones completas para diseñar e implementar una infraestructura de clave pública (PKI) mediante el uso de AD CS. Se recomienda que revise la documentación de AD CS y PKI diseño antes de implementar las tecnologías en esta guía.   

## <a name="bkmk_tech"></a>Información general de la tecnología  
A continuación se muestran información general de la tecnología de AD CS y el servidor Web (IIS).  

### <a name="active-directory-certificate-services"></a>Servicios de certificados de Active Directory  
AD CS en Windows Server 2016 proporciona servicios personalizables para crear y administrar los certificados X.509 que se usan en los sistemas de seguridad de software que emplean tecnologías de clave pública. Las organizaciones pueden usar los AD CS para mejorar la seguridad al enlazar la identidad de una persona, dispositivo o servicio a una clave pública correspondiente. AD CS también incluye características que permiten administrar la inscripción de certificados y revocación de certificados en una variedad de entornos escalables.  

Para obtener más información, consulta [introducción de servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx) y [Guía de diseño de infraestructura de clave pública ](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).  

### <a name="web-server-iis"></a>Servidor Web (IIS)  

El rol de servidor Web (IIS) en Windows Server 2016, proporciona una plataforma segura fácil de administrar, modular y extensible para hospedar los sitios Web, servicios y aplicaciones de forma segura. Con IIS, puede compartir información con los usuarios en Internet, intranet o una extranet. IIS es una plataforma web unificado que se integra IIS, ASP.NET, servicios FTP, PHP y Windows Communication Foundation (WCF).  

Para obtener más información, consulta [información general de servidor Web (IIS)](https://technet.microsoft.com/library/hh831725.aspx).  
