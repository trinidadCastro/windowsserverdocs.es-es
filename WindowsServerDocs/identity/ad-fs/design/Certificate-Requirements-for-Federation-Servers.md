---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisitos de certificado para servidores de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9154e168fa03b08177dd0125fcbc1707d8e5594f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853218"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de federación

En cualquier Servicios de federación de Active Directory (AD FS) \(AD FS el diseño de\), deben usarse varios certificados para proteger la comunicación y facilitar las autenticaciones de usuario entre los clientes de Internet y los servidores de Federación. Cada servidor de Federación debe tener un certificado de comunicación de servicio y un token\-certificado de firma antes de que pueda participar en las comunicaciones de AD FS. En la tabla siguiente se describen los tipos de certificado asociados con el servidor de Federación.  
  
|Tipo de certificado|Descripción|  
|--------------------|---------------|  
|Certificado de firma de\-de tokens|Un token\-certificado de firma es un certificado X509. Los servidores de Federación utilizan pares de clave privada\/pública para firmar digitalmente todos los tokens de seguridad que producen. Esto incluye la firma de metadatos de federación publicados y solicitudes de resolución de artefactos.<p>Puede tener varios certificados de firma de\-de tokens configurados en el complemento de administración de AD FS\-en para permitir la sustitución de certificados cuando un certificado está a la expiración. De forma predeterminada, se publican todos los certificados de la lista, pero AD FS usa únicamente el token principal\-certificado de firma para firmar los tokens. Todos los certificados que selecciones deben tener su propia clave privada.<p>Para obtener más información, consulta [Certificados de firma de tokens](Token-Signing-Certificates.md) y [Agregar un certificado de firma de tokens](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicación de servicios|Los servidores de Federación usan un certificado de autenticación de servidor, también conocido como comunicación de servicio para Windows Communication Foundation \(WCF\) seguridad de mensajes. De forma predeterminada, se trata del mismo certificado que usa un servidor de Federación como Capa de sockets seguros \(certificado\) SSL en Internet Information Services \(IIS\). **Nota:** El\-de administración de AD FS en se refiere a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.<p>Para obtener más información, vea [certificados de comunicaciones de servicio](Service-Communications-Certificates.md) y [establecer un certificado de comunicaciones de servicio](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<p>Dado que el certificado de comunicación de servicio debe ser de confianza para los equipos cliente, se recomienda usar un certificado firmado por una entidad de certificación de confianza \(CA\). Todos los certificados que selecciones deben tener su propia clave privada.|  
|Capa de sockets seguros certificado \(SSL\)|Los servidores de federación usan un certificado SSL para proteger el tráfico de servicios web para la comunicación SSL con clientes web y con los servidores proxy de federación.<p>Dado que el certificado SSL debe ser de confianza para los equipos cliente, recomendamos usar un certificado que lleve la firma de una CA de confianza. Todos los certificados que selecciones deben tener su propia clave privada.|  
|Certificado de descifrado de\-de tokens|Este certificado se usa para descifrar los tokens recibidos por este servidor de Federación.<p>Es posible disponer de varios certificados de descifrado. Esto hace posible que un servidor de Federación de recursos pueda descifrar los tokens que se emiten con un certificado anterior después de establecer un nuevo certificado como el certificado de descifrado principal. Todos los certificados se pueden usar para el descifrado, pero solo el token primario\-el certificado de descifrado se publica realmente en los metadatos de Federación. Todos los certificados que selecciones deben tener su propia clave privada.<p>Para obtener más información, consulte [Agregar un certificado de descifrado de tokens](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Puede solicitar e instalar un certificado SSL o un certificado de comunicación de servicio solicitando un certificado de comunicación de servicio a través de Microsoft Management Console \(MMC\) Snap\-en para IIS. Para obtener más información general sobre el uso de certificados SSL, vea [iis 7,0: configurar capa de sockets seguros en iis 7,0](https://go.microsoft.com/fwlink/?LinkID=108544) e [IIS 7,0: configurar certificados de servidor en IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> En AD FS puede cambiar el algoritmo hash seguro \(el nivel SHA\) que se usa para las firmas digitales a SHA\-1 o SHA\-256 \(\)más seguro. AD FSdoes no admite el uso de certificados con otros métodos hash, como MD5 \(el algoritmo hash predeterminado que se usa con la herramienta de línea de\-de comandos Makecert. exe\). Como práctica recomendada de seguridad, se recomienda usar SHA\-256 \(, que se establece de forma predeterminada\) para todas las firmas. Se recomienda el uso de SHA\-1 solo en escenarios en los que se debe interoperar con un producto que no admite las comunicaciones que usan SHA\-256, como un producto no\-Microsoft o AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinar la estrategia de CA  
AD FS no requiere que los certificados los emita una entidad de certificación. Sin embargo, el certificado SSL \(el certificado que también se usa de forma predeterminada como el certificado de comunicaciones de servicio\) debe ser de confianza para los clientes de AD FS. Se recomienda no usar certificados\-firmados para estos tipos de certificado.  
  
> [!IMPORTANT]  
> El uso de los certificados SSL\-firmados en un entorno de producción puede permitir que un usuario malintencionado en una organización de asociado de cuenta tome el control de los servidores de Federación en una organización del asociado de recurso. Este riesgo de seguridad existe porque los certificados firmados por\-propios son certificados raíz. Deben agregarse al almacén raíz de confianza de otro servidor de Federación \(por ejemplo, el servidor de Federación de recursos\), que puede dejar ese servidor vulnerable a ataques.  
  
Después de recibir un certificado de una CA, asegúrate de que todos los certificados se importen al almacén de certificados personales del equipo local. Puede importar certificados al almacén personal con el complemento certificados de MMC\-en.  
  
Como alternativa al uso del complemento de certificados\-en, también puede importar el certificado SSL con el\-complemento Administrador de IIS, en el momento de asignar el certificado SSL al sitio web predeterminado. Para obtener más información, vea [importar un certificado de autenticación de servidor al sitio web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar el software de AD FS en el equipo que se convertirá en el servidor de Federación, asegúrese de que ambos certificados se encuentren en el almacén de certificados personales del equipo local y que el certificado SSL esté asignado al sitio web predeterminado. Para obtener más información acerca del orden de las tareas necesarias para configurar un servidor de Federación, consulte [lista de comprobación: configuración de un servidor de Federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Considera qué certificados se obtendrán de una CA pública y cuáles de una CA corporativa en función de tus requisitos de seguridad y de tu presupuesto. La siguiente imagen muestra los emisores de CA recomendados según el tipo de certificado. Esta recomendación refleja el mejor enfoque de\-prácticas con respecto a la seguridad y el costo.  
  
![requisitos de certificado](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de revocación de certificados  
Si un certificado que utilizas tiene listas de revocación de certificados (CRL), el servidor con el certificado configurado debe poder establecer contacto con el servidor que distribuye las CRL.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
