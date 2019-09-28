---
ms.assetid: 9831b421-8fb7-4e15-ac27-c013cbca6d05
title: Requisitos de certificado para servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7eeff82ef3311f18c8252c44c96310fcf3c18217
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359184"
---
# <a name="certificate-requirements-for-federation-servers"></a>Requisitos de certificado para servidores de federación

En cualquier Servicios de federación de Active Directory (AD FS) @no__t 0AD FS @ no__t-1, deben usarse varios certificados para proteger la comunicación y facilitar las autenticaciones de usuario entre los clientes de Internet y los servidores de Federación. Cada servidor de Federación debe tener un certificado de comunicación de servicio y un certificado de token @ no__t-0signing para poder participar en las comunicaciones AD FS. En la tabla siguiente se describen los tipos de certificado asociados con el servidor de Federación.  
  
|Tipo de certificado|Descripción|  
|--------------------|---------------|  
|Token @ no__t-0signing certificado|Un certificado de token @ no__t-0signing es un certificado X509. Los servidores de Federación utilizan pares de claves públicos @ no__t-0private asociados para firmar digitalmente todos los tokens de seguridad que producen. Esto incluye la firma de metadatos de federación publicados y solicitudes de resolución de artefactos.<br /><br />Puede tener varios certificados de token @ no__t-0signing configurados en el AD FS de administración de configuración @ no__t-1in para permitir la sustitución de certificados cuando un certificado está a la expiración. De forma predeterminada, se publican todos los certificados de la lista, pero el certificado del token principal @ no__t-0signing lo usa AD FS para firmar los tokens. Todos los certificados que selecciones deben tener su propia clave privada.<br /><br />Para obtener más información, consulte [Token-Signing Certificates](Token-Signing-Certificates.md) y [Add a Token-Signing Certificate](../../ad-fs/deployment/Add-a-Token-Signing-Certificate.md).|  
|Certificado de comunicación de servicios|Los servidores de Federación usan un certificado de autenticación de servidor, también conocido como comunicación de servicio para la seguridad del mensaje Windows Communication Foundation \(WCF @ no__t-1. De forma predeterminada, se trata del mismo certificado que usa un servidor de Federación como el certificado Capa de sockets seguros \(SSL @ no__t-1 en Internet Information Services \(IIS @ no__t-3. **Nota:** El complemento de administración de AD FS @ no__t-0in hace referencia a los certificados de autenticación de servidor para los servidores de Federación como certificados de comunicación de servicio.<br /><br />Para obtener más información, vea [certificados de comunicaciones de servicio](Service-Communications-Certificates.md) y [establecer un certificado de comunicaciones de servicio](../../ad-fs/deployment/Set-a-Service-Communications-Certificate.md).<br /><br />Dado que el certificado de comunicación de servicio debe ser de confianza para los equipos cliente, se recomienda usar un certificado firmado por una entidad de certificación de confianza \(CA @ no__t-1. Todos los certificados que selecciones deben tener su propia clave privada.|  
|Capa de sockets seguros \(SSL @ no__t-1|Los servidores de federación usan un certificado SSL para proteger el tráfico de servicios web para la comunicación SSL con clientes web y con los servidores proxy de federación.<br /><br />Dado que el certificado SSL debe ser de confianza para los equipos cliente, recomendamos usar un certificado que lleve la firma de una CA de confianza. Todos los certificados que selecciones deben tener su propia clave privada.|  
|Token @ no__t-0decryption certificado|Este certificado se usa para descifrar los tokens recibidos por este servidor de Federación.<br /><br />Es posible disponer de varios certificados de descifrado. Esto hace posible que un servidor de Federación de recursos pueda descifrar los tokens que se emiten con un certificado anterior después de establecer un nuevo certificado como el certificado de descifrado principal. Todos los certificados se pueden usar para el descifrado, pero solo el certificado del token principal @ no__t-0decrypting se publica realmente en los metadatos de Federación. Todos los certificados que selecciones deben tener su propia clave privada.<br /><br />Para obtener más información, consulte [Agregar un certificado de descifrado de tokens](../../ad-fs/deployment/Add-a-Token-Decrypting-Certificate.md).|  
  
Puede solicitar e instalar un certificado SSL o un certificado de comunicación de servicio solicitando un certificado de comunicación de servicio a través de Microsoft Management Console \(MMC @ no__t-1 Snap @ no__t-2in para IIS. Para obtener más información general sobre el uso de certificados SSL, consulte @no__t 0IIS 7,0: Configuración de Capa de sockets seguros en IIS 7.0 @ no__t-0 y [IIS 7,0: Configuración de certificados de servidor en IIS 7.0 @ no__t-0.  
  
> [!NOTE]  
> En AD FS puede cambiar el \(nivel Sha\) de algoritmo hash seguro que se usa para las firmas digitales a Sha\-1 o Sha\-256 \(de forma más\)segura. AD FSdoes no admite el uso de certificados con otros métodos hash, como el algoritmo hash predeterminado MD5 \(The que se usa con el comando Makecert. exe @ no__t-1line Tool @ no__t-2. Como práctica recomendada de seguridad, se recomienda usar Sha\-256 \(, que se establece de forma predeterminada\) para todas las firmas. Se recomienda usar SHA @ no__t-01 solo en escenarios en los que se debe interoperar con un producto que no admite las comunicaciones que usan SHA @ no__t-1256, como un producto que no es @ no__t-2Microsoft o un AD FS 1. *x*.  
  
## <a name="determining-your-ca-strategy"></a>Determinar la estrategia de CA  
AD FS no requiere que los certificados los emita una entidad de certificación. Sin embargo, el certificado SSL \(The que también se usa de forma predeterminada como el certificado de comunicaciones de servicio @ no__t-1 debe ser de confianza para los clientes de AD FS. Se recomienda no usar certificados Self @ no__t-0signed para estos tipos de certificado.  
  
> [!IMPORTANT]  
> El uso de los certificados SSL de @ no__t-0signed en un entorno de producción puede permitir que un usuario malintencionado de una organización de asociado de cuenta tome el control de los servidores de Federación en una organización del asociado de recurso. Este riesgo de seguridad existe porque los certificados Self @ no__t-0signed son certificados raíz. Deben agregarse al almacén raíz de confianza de otro servidor de Federación @no__t ejemplo 0for, el servidor de Federación de recursos @ no__t-1, que puede dejar que el servidor sea vulnerable a los ataques.  
  
Después de recibir un certificado de una CA, asegúrate de que todos los certificados se importen al almacén de certificados personales del equipo local. Puede importar certificados al almacén personal con el complemento\-certificados de MMC.  
  
Como alternativa al uso de los certificados Snap @ no__t-0in, también puede importar el certificado SSL con el administrador de IIS Snap @ no__t-1in en el momento de asignar el certificado SSL al sitio web predeterminado. Para obtener más información, vea [importar un certificado de autenticación de servidor al sitio web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).  
  
> [!NOTE]  
> Antes de instalar el software de AD FS en el equipo que se convertirá en el servidor de Federación, asegúrese de que ambos certificados se encuentren en el almacén de certificados personales del equipo local y que el certificado SSL esté asignado al sitio web predeterminado. Para obtener más información sobre el orden de las tareas necesarias para configurar un servidor de Federación, consulte @no__t 0Checklist: Configuración de un servidor de Federación @ no__t-0.  
  
Considera qué certificados se obtendrán de una CA pública y cuáles de una CA corporativa en función de tus requisitos de seguridad y de tu presupuesto. La siguiente imagen muestra los emisores de CA recomendados según el tipo de certificado. Esta recomendación refleja un mejor enfoque de @ no__t-0practice con respecto a la seguridad y el costo.  
  
![requisitos de certificado](media/adfs2_fedserver_certstory_1.png)  
  
## <a name="certificate-revocation-lists"></a>Listas de revocación de certificados  
Si un certificado que utilizas tiene listas de revocación de certificados (CRL), el servidor con el certificado configurado debe poder establecer contacto con el servidor que distribuye las CRL.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
