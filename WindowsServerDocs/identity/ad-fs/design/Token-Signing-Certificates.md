---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificados de firma de token
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 84e106ec87861960d7de651b4aaa0e88c952aa79
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="token-signing-certificates"></a>Certificados de firma de token

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federación requieren certificados de firma de token\ para evitar que los atacantes modifiquen o contra la falsificación de tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. La clave pública y private\ emparejamiento es usar con los certificados de firma de token\ es el mecanismo más importante de validación de las asociaciones federados porque estas claves comprobar que un token de seguridad fue emitido por un servidor de federación de asociado válido y que no se ha modificado el token durante el tránsito.  
  
## <a name="token-signing-certificate-requirements"></a>Firma de Token\ requisitos de certificados  
Un certificado de firma token\ debe cumplir los siguientes requisitos para que funcione con AD FS:  
  
-   Para que un certificado de firma token\ volver a iniciar sesión un token de seguridad, el certificado de firma token\ debe contener una clave privada.  
  
-   La cuenta de servicio de AD FS debe tener acceso a la clave privada del certificado de firma de token\ en el almacén personal del equipo local. Esto se ocupa de instalación. También puedes usar la administración de AD FS en snap\ para asegurarte de este acceso si posteriormente cambias el certificado de firma token\.  
  
> [!NOTE]  
> Es un procedimiento recomendado de infraestructura de clave pública \(PKI\) no compartir la clave privada para varios propósitos. Por lo tanto, no utilice el certificado de comunicación de servicio que ha instalado en el servidor de federación como el certificado de firma token\.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Cómo se usan los certificados de firma de token\ a través de partners  
Cada certificado de firma de token\ contiene claves privadas cifradas y claves públicas que se usan para firmar digitalmente \ (mediante la key\ privada) un token de seguridad. Más adelante, después de que se reciben un servidor de federación de partner, estas claves validan la autenticidad \ (mediante la key\ pública) del token de seguridad cifrado.  
  
Dado que cada token de seguridad está firmado digitalmente por el asociado de cuenta, el asociado de recurso puede comprobar que el token de seguridad en realidad fue emitido por el asociado de cuenta y que no se ha modificado. La parte de la clave pública del certificado de un partner token\ cantando comprueba firmas digitales. Después de comprobar la firma, el servidor de federación de recursos genera su propio token de seguridad para su organización y firma el token de seguridad con su propio certificado de firma de token\.  
  
Para entornos de partner de federación, cuando se ha emitido el certificado de firma token\ por una entidad de certificación, asegúrate de que:  
  
1.  El certificado revocación listas \(CRLs\) del certificado son accesibles para usuarios de confianza y servidores Web que confíe en el servidor de federación.  
  
2.  El certificado de CA de raíz es de confianza para los usuarios de confianza y los servidores Web que confíe en el servidor de federación.  
  
El servidor Web en el partner de recursos usa la clave pública del certificado de firma de token\ para comprobar que el token de seguridad está firmado por el servidor de federación de recursos. El servidor Web, a continuación, permite el acceso adecuado para el cliente.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Consideraciones de implementación de certificados de firma de token\  
Al implementar el primer servidor de federación de una nueva instalación de AD FS, debe obtener un certificado de firma token\ e instalarlo en el almacén de certificados personal del equipo local en el servidor de federación. Puedes obtener una firma token\ certificado por una solicitud de una empresa, CA o una CA pública o creando un certificado de firma de self\.  
  
Cuando implementas un conjunto de AD FS, certificados de firma de token\ se instalan de forma diferente, dependiendo de cómo crear la granja de servidores.  
  
Existen dos opciones de granja de servidor que puede tener en cuenta cuando obtengas certificados de firma de token\ para la implementación:  
  
-   Una clave privada de un certificado de firma de token\ se comparte entre todos los servidores de federación de una granja de servidores.  
  
    En un entorno de granja de servidores de federación, te recomendamos que todos los servidores de federación comparten el mismo certificado de firma de token\ de \(or reuse\). Puedes instalar un certificado de firma token\ único de una CA en un servidor de federación y luego exportar la clave privada, como el certificado emitido está marcado como puede exportar.  
  
    Como se muestra en la siguiente ilustración, se puede compartir la clave privada de un único certificado de firma de token\ a todos los servidores de federación de un conjunto. Esta opción, en comparación con el siguiente "único token\ certificado de firma" opción, reduce los costos si vas a obtener un certificado de firma token\ de una CA pública.  
  
![firma de tokens](media/adfs2_fedserver_certstory_3.gif)  
  
-   Hay un certificado de firma token\ único para cada servidor de federación en una granja de servidores.  
  
    Cuando usas varias, certificados únicos en toda la granja de servidores, cada servidor ese conjunto firma tokens con su propia clave privada único.  
  
    Como se muestra en la siguiente ilustración, se puede obtener un certificado de firma token\ independiente por cada servidor de federación único de la batería. Esta opción es más costosa si vas a obtener los certificados de firma de token\ de una CA pública.  
  
![firma de tokens](media/adfs2_fedserver_certstory_4.gif)  
  
Para obtener información sobre cómo instalar un certificado al usar servicios de certificados de Microsoft como de la CA de empresa, consulta [IIS 7.0: crear un certificado de servidor de dominio en IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Para obtener información sobre cómo instalar un certificado de una CA pública, vea [IIS 7.0: solicitar un certificado de servidor de Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Para obtener información sobre cómo instalar un certificado de firma de self\, vea [IIS 7.0: crear un certificado de servidor Self\-Signed en IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
