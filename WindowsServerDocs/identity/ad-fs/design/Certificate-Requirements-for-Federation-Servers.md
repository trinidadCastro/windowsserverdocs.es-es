---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: "Requisitos de certificado para los servidores de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 369c0e9e7ab1ef25baee1c35379cc66b886f20d8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para los servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En cualquier diseño \(AD FS\) de los servicios de federación de Active Directory, varios certificados deben usarse para proteger la comunicación y facilitar la autenticación de usuario entre los clientes de Internet y servidores de federación. Cada servidor de federación debe tener un certificado de comunicación de servicio y un certificado de firma token\ para que pueda participar en las comunicaciones de AD FS. La siguiente tabla describen los tipos de certificados están asociados con el servidor de federación.  
  
|Tipo de certificado|Descripción|  
|--------------------|---------------|  
|Certificado de firma de Token\|Un certificado de firma token\ es una X509 certificado. Los servidores de federación usan pares de claves privada y public\ asociados para firmar digitalmente todos los tokens de seguridad que se producen. Esto incluye la firma de solicitudes de resolución de metadatos y artefacto federación publicado.<br /><br />Puedes tener varios certificados de firma de token\ configurados en la administración de FS anuncios en snap\ para permitir cuando un certificado está a punto de expirar – la sustitución del certificado. De manera predeterminada, todos los certificados de la lista se publican, pero se usa solo el certificado de firma token\ principal por AD FS firmar tokens. Todos los certificados que selecciones deben tener una clave privada correspondiente.<br /><br />Para obtener más información, consulta [certificados de firma de Token](Token-Signing-Certificates.md) y [agregar un certificado de firma de tokens](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicación de servicio|Los servidores de federación usan un certificado de autenticación de servidor, también conocido como una comunicación de servicio para Windows Communication Foundation \(WCF\) seguridad de los mensajes. De manera predeterminada, este es el mismo certificado de un servidor de federación se usa como el certificado de capa de Sockets seguros \(SSL\) en Internet Information Services \(IIS\). **Nota:** la administración de AD FS en snap\ hace referencia a los certificados de autenticación de servidor para servidores de federación como certificados de comunicación de servicio.<br /><br />Para obtener más información, consulta [certificados de comunicaciones de servicio](Service-Communications-Certificates.md) y [establece un certificado de comunicaciones de servicio](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Dado que el certificado de comunicación de servicio debe ser de confianza para los equipos cliente, te recomendamos que uses un certificado que esté firmado por una entidad de certificación de confianza \(CA\). Todos los certificados que selecciones deben tener una clave privada correspondiente.|  
|Certificado \(SSL\) capa de Sockets seguras|Los servidores de federación usan un certificado SSL para proteger el tráfico de servicios Web para la comunicación de SSL con los clientes Web y con servidores proxy de servidor de federación.<br /><br />Dado que el certificado SSL debe ser de confianza para los equipos cliente, te recomendamos que uses un certificado que esté firmado por una entidad de certificación de confianza. Todos los certificados que selecciones deben tener una clave privada correspondiente.|  
|Certificado de descifrado de Token\|Este certificado se usa para descifrar tokens recibidos por este servidor de federación.<br /><br />Puedes tener varios certificados de descifrado. Esto permite un servidor de federación de recursos poder descifrar tokens emitidos con un certificado anterior después de establece un nuevo certificado como el certificado de descifrado principal. Todos los certificados pueden usarse para el descifrado, pero solo el certificado de descifrado de token\ principal realmente se publica en los metadatos de federación. Todos los certificados que selecciones deben tener una clave privada correspondiente.<br /><br />Para obtener más información, consulta [agregar un certificado de descifrado de Token](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Puedes solicitar e instalar un certificado SSL o un certificado de comunicación de servicio al solicitar un certificado de comunicación de servicio a través de la \(MMC\) snap\ Microsoft Management Console-en IIS. Para obtener más información general sobre el uso de certificados SSL, consulta [IIS 7.0: configuración de capa de Sockets seguros en IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108544) y [IIS 7.0: configuración de certificados de servidor en IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> En AD FS puede cambiar el nivel de algoritmo Hash seguro \(SHA\) que se usa para las firmas digitales para \(more secure\) SHA\-1 o SHA\-256. AD FSdoes no admite el uso de certificados con otros métodos de hash, como MD5 \ (el algoritmo hash predeterminado que se usa con la tool\ Web\ línea Makecert.exe). Como procedimiento recomendado de seguridad, te recomendamos que uses SHA\-256 \ (que se establece por default\) para todas las firmas. Se recomienda SHA\-1 para uso exclusivo en escenarios en los que deben interoperar con un producto que no es compatible con las comunicaciones mediante SHA\-256, como un producto de Microsoft non\ o AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinar la estrategia de entidad emisora de certificados  
AD FS no requiere que los certificados se emiten por una entidad de certificación. Sin embargo, el certificado SSL \ (es decir, el certificado que también se usa de manera predeterminada como la certificate\ de comunicaciones de servicio) debe ser de confianza para los clientes de AD FS. Te recomendamos que uses no certificados firmados self\ para estos tipos de certificados.  
  
> [!IMPORTANT]  
> Uso de firma de self\, certificados SSL en un entorno de producción pueden permitir que a un usuario malintencionado en una organización de partner de cuenta para tomar el control de los servidores de federación en una organización de partner de recursos. Este riesgo de seguridad existe porque certificados firmados self\ son certificados raíz. Deben agregarse a la tienda de la raíz de confianza de otro servidor de federación \ (por ejemplo, la server\ de federación de recursos), que puede dejar ese servidor vulnerable a ataques.  
  
Después de recibir un certificado de una entidad de certificación, asegúrate de que todos los certificados se importan en el almacén de certificados personales del equipo local. Puedes importar certificados al almacén personal con MMC snap\ en certificados.  
  
Como alternativa al uso de los certificados de snap\, también puedes importar el certificado SSL en el Administrador de IIS snap\ al tiempo que va a asignar el certificado SSL para el sitio Web predeterminado. Para obtener más información, consulta [importar un certificado de autenticación de servidor en el sitio Web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar el software de AD FS en el equipo que se convertirá en el servidor de federación, asegúrate de que ambos certificados están en el almacén de certificados personal del equipo Local y que el certificado SSL se asigna al sitio Web predeterminado. Para obtener más información sobre el orden de las tareas que son obligatorias para configurar un servidor de federación, consulta [lista de comprobación: configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
Según los requisitos de seguridad y presupuesto, analiza detenidamente cuál de los certificados se obtendrán por un público CA o una CA de empresa. La figura siguiente muestra a los emisores de CA recomendados para un determinado tipo de certificado. Esta recomendación refleja best\ práctica en relación con la seguridad y costo.  
  
![requisitos de certificación](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de revocación de certificados  
Si ningún certificado que usas con CRL, el servidor con el certificado configurado debe poder ponerte en contacto con el servidor que distribuye las CRL.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
