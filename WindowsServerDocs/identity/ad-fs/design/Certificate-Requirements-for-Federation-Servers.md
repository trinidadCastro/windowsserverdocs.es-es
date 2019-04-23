---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisitos de certificado para servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827106"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En los servicios de federación de Active Directory \(AD FS\) diseño, deben utilizarse varios certificados para proteger la comunicación y facilitar las autenticaciones de usuario entre los clientes de Internet y los servidores de federación. Cada servidor de federación debe tener un certificado de comunicación de servicio y un token de\-certificados de firma para que pueda participar en comunicaciones con AD FS. En la tabla siguiente se describe los tipos de certificados que están asociados con el servidor de federación.  
  
|Tipo de certificado|Descripción|  
|--------------------|---------------|  
|Token\-certificado de firma|Un token\-certificado de firma es un X509 certificado. Los servidores de federación usan público asociado\/pares de claves privados para firmar digitalmente todos los tokens de seguridad que producen. Esto incluye la firma de metadatos de federación publicados y solicitudes de resolución de artefactos.<br /><br />Puede tener varios token\-firma los certificados configurados en el complemento Administración de AD FS\-en para permitir la sustitución de certificados cuando un certificado está a punto de expirar. De forma predeterminada, se publican todos los certificados en la lista, pero solo el token primario\-certificado de firma se usa para firmar los tokens de AD FS. Todos los certificados que selecciones deben tener su propia clave privada.<br /><br />Para obtener más información, consulte [Token-Signing Certificates](Token-Signing-Certificates.md) y [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicación de servicios|Los servidores de federación utilizan un certificado de autenticación de servidor, también conocido como una comunicación de servicio de Windows Communication Foundation \(WCF\) seguridad del mensaje. De forma predeterminada, este es el mismo certificado que usa un servidor de federación como la capa de Sockets seguros \(SSL\) certificado en Internet Information Services \(IIS\). **Nota:** El complemento de administración de AD FS\-en hace referencia a certificados de autenticación de servidor para los servidores de federación como certificados de comunicación de servicio.<br /><br />Para obtener más información, consulte [certificados de comunicaciones de servicio](Service-Communications-Certificates.md) y [establecer un certificado de comunicaciones de servicio](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Dado que el certificado de comunicación de servicio debe ser de confianza por los equipos cliente, se recomienda usar un certificado firmado por una entidad de certificación de confianza \(CA\). Todos los certificados que selecciones deben tener su propia clave privada.|  
|Capa de Sockets seguros \(SSL\) certificado|Los servidores de federación usan un certificado SSL para proteger el tráfico de servicios web para la comunicación SSL con clientes web y con los servidores proxy de federación.<br /><br />Dado que el certificado SSL debe ser de confianza para los equipos cliente, recomendamos usar un certificado que lleve la firma de una CA de confianza. Todos los certificados que selecciones deben tener su propia clave privada.|  
|Token\-certificado de descifrado|Este certificado se usa para descifrar tokens recibidos por este servidor de federación.<br /><br />Es posible disponer de varios certificados de descifrado. Esto hace posible que un servidor de federación de recursos poder descifrar los tokens emitidos por un certificado antiguo después de establece un nuevo certificado como certificado de descifrado principal. Todos los certificados se pueden usar para el descifrado, pero solo el token primario\-certificado de descifrado se publica en los metadatos de federación. Todos los certificados que selecciones deben tener su propia clave privada.<br /><br />Para obtener más información, consulte [agregar un certificado de descifrado de tokens](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Puede solicitar e instalar un certificado SSL o un certificado de comunicación de servicio al solicitar un certificado de comunicación de servicio a través de la consola de administración de Microsoft \(MMC\) ajustar\-en IIS. Para obtener información general sobre el uso de certificados SSL, consulte [IIS 7.0: Configuración de Sockets seguros capa en IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) y [IIS 7.0: Configuración de certificados de servidor en IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> En AD FS puede cambiar el algoritmo Hash seguro \(SHA\) nivel que se usa para las firmas digitales para cualquier SHA\-1 o SHA\-256 \(más seguro\). AD FSdoes no admite el uso de certificados con otros métodos hash, como MD5 \(el algoritmo hash predeterminado que se usa con el comando Makecert.exe\-herramienta de línea de\). Como práctica recomendada de seguridad, le recomendamos que use SHA\-256 \(que se establece de forma predeterminada\) para todas las firmas. SHA\-1 se recomienda para su uso solo en los casos en que se debe interoperar con un producto que no es compatible con las comunicaciones mediante SHA\-256, por ejemplo, una que no sean\-producto de Microsoft o AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinar la estrategia de CA  
AD FS no requiere que los certificados se emiten por una entidad de certificación. Sin embargo, el certificado SSL \(el certificado que también se usa de forma predeterminada como el certificado de comunicaciones de servicio\) debe ser de confianza por los clientes de AD FS. Se recomienda que no use self\-certificados autofirmados para estos tipos de certificados.  
  
> [!IMPORTANT]  
> Uso de self\-firmado, certificados SSL en un entorno de producción pueden permitir que un usuario malintencionado en una organización del asociado de cuenta para tomar el control de los servidores de federación en una organización del asociado de recurso. Este riesgo de seguridad existe porque self\-certificados autofirmados son certificados raíz. Deben agregarse al almacén raíz de confianza de otro servidor de federación \(por ejemplo, el servidor de federación de recursos\), que puede hacer que el servidor sea vulnerable a ataques.  
  
Después de recibir un certificado de una CA, asegúrate de que todos los certificados se importen al almacén de certificados personales del equipo local. Puede importar certificados al almacén personal con el complemento MMC certificados\-en.  
  
Como alternativa al uso de los certificados de ajuste\-, también puede importar el certificado SSL con el complemento Administrador de IIS\-en el momento en que se asigna la SSL de certificado para el sitio Web predeterminado. Para obtener más información, consulte [importar un certificado de autenticación de servidor al sitio Web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar el software AD FS en el equipo que se convertirá en el servidor de federación, asegúrese de que ambos certificados se encuentren en el almacén de certificados personales del equipo Local y que el certificado SSL se asigna al sitio Web predeterminado. Para obtener más información acerca del orden de las tareas que son necesarios para configurar un servidor de federación, consulte [lista de comprobación: Configuración de un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Considera qué certificados se obtendrán de una CA pública y cuáles de una CA corporativa en función de tus requisitos de seguridad y de tu presupuesto. La siguiente imagen muestra los emisores de CA recomendados según el tipo de certificado. Esta recomendación refleja una mejor\-practicar relativos a la seguridad y el costo.  
  
![requisitos del certificado](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de revocación de certificados  
Si un certificado que utilizas tiene listas de revocación de certificados (CRL), el servidor con el certificado configurado debe poder establecer contacto con el servidor que distribuye las CRL.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
