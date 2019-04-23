---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificados de firma de tokens
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a047b94906cf703bb934c93f517b8874af91e092
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864066"
---
# <a name="token-signing-certificates"></a>Certificados de firma de tokens

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servidores de federación requieren el token\-certificados de firma para evitar que los atacantes modifiquen o falsifiquen los tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. Privado\/emparejamiento de clave pública se utiliza con el token\-certificados de firma es el mecanismo de validación más importante de cualquier asociación federada porque estas claves comprobación que un token de seguridad fue emitido por un socio válido servidor de federación y que el token no se ha modificado durante el tránsito.  
  
## <a name="token-signing-certificate-requirements"></a>Token\-requisitos de certificado de firma  
Un token\-certificado de firma debe cumplir los siguientes requisitos para que funcione con AD FS:  
  
-   Para obtener un token\-certificado para firmar correctamente un token de seguridad, el token de firma\-certificado de firma debe contener una clave privada.  
  
-   La cuenta de servicio de AD FS debe tener acceso al token\-firma de clave privada del certificado en el almacén personal del equipo local. Esto se ocupa del programa de instalación. También puede usar el complemento Administración de AD FS\-para asegurarse de este acceso si se cambia posteriormente el token\-certificado de firma.  
  
> [!NOTE]  
> Es una infraestructura de clave pública \(PKI\) recomendado para no compartir la clave privada para fines diferentes. Por lo tanto, no use el certificado de comunicación de servicio que ha instalado en el servidor de federación como el token\-certificado de firma.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Cómo token\-se usan certificados de firma a través de asociados  
Cada token\-certificado de firma contiene claves privadas cifradas y claves públicas que se utilizan para firmar digitalmente \(mediante la clave privada\) un token de seguridad. Más adelante, después de que se reciben por un servidor de federación del asociado, estas claves validan la autenticidad \(por medio de la clave pública\) del token de seguridad cifrada.  
  
Dado que cada token de seguridad está firmado digitalmente por el asociado de cuenta, el asociado de recurso puede comprobar que el token de seguridad fue emitido realmente por el asociado de cuenta y que no se ha modificado. Las firmas digitales comprueban la parte de la clave pública del token de un socio\-certificado de firma. Una vez que se comprueba la firma, el servidor de federación de recursos genera su propio token de seguridad para su organización y firma el token de seguridad con su propio token\-certificado de firma.  
  
Para entornos de asociado de federación, cuando el token\-se emitió el certificado de firma de una entidad de certificación, asegúrese de que:  
  
1.  Las listas de revocación de certificados \(CRL\) del certificado son accesibles a los usuarios de confianza y servidores Web que confíe en el servidor de federación.  
  
2.  Es el certificado de CA raíz de confianza por los usuarios de confianza y servidores Web que confíe en el servidor de federación.  
  
El servidor Web en el asociado de recurso utiliza la clave pública del token\-certificado de firma para comprobar si el token de seguridad está firmado por el servidor de federación de recursos. El servidor Web, a continuación, permite el acceso adecuado al cliente.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Consideraciones de implementación de token\-certificados de firma  
Al implementar el primer servidor de federación en una nueva instalación de AD FS, debe obtener un token\-de firma de certificado e instalarlo en el almacén de certificados personales del equipo local en ese servidor de federación. Puede obtener un token\-certificados de firma de una solicitud de una empresa entidad emisora de certificados o una entidad de certificación pública o mediante la creación de un autoservicio\-certificado autofirmado.  
  
Al implementar una granja de AD FS, token\-certificados de firma se instalan de forma diferente, dependiendo de cómo crear la granja de servidores.  
  
Hay dos opciones de la granja de servidor que puede tener en cuenta cuando obtienen token\-certificados de firma para la implementación:  
  
-   Una clave privada desde un símbolo (token)\-certificado de firma se comparte entre todos los servidores de federación en una granja de servidores.  
  
    En un entorno de granja de servidores de federación, se recomienda que todos los servidores de federación compartan \(o reutilizar\) el mismo token\-certificado de firma. Puede instalar un único token\-firma de certificado de una entidad de certificación en un servidor de federación y, a continuación, exportar la clave privada, siempre y cuando el certificado emitido está marcado como exportable.  
  
    Como se muestra en la siguiente ilustración, la clave privada de una sola token\-se puede compartir el certificado de firma para todos los servidores de federación en una granja de servidores. Esta opción, en comparación con lo siguiente "token único\-certificado de firma" opción: reduce los costos si va a obtener un token\-firma de certificado de una CA pública.  
  
![firma de tokens](media/adfs2_fedserver_certstory_3.gif)  
  
-   Hay un único token\-firma de certificado para cada servidor de federación en una granja de servidores.  
  
    Cuando use varias, certificados únicos en toda la granja de servidores, cada servidor de la granja firma tokens con su propia clave privada único.  
  
    Como se muestra en la siguiente ilustración, puede obtener un token independiente\-firma de certificado para cada servidor de federación único en la granja de servidores. Esta opción es más cara si va a obtener el token\-certificados desde una CA pública de firma.  
  
![firma de tokens](media/adfs2_fedserver_certstory_4.gif)  
  
Para obtener información acerca de cómo instalar un certificado al usar servicios de Certificate Server de Microsoft como la CA de empresa, consulte [IIS 7.0: Crear un certificado de servidor de dominio en IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Para obtener información acerca de cómo instalar un certificado de una CA pública, consulte [IIS 7.0: Solicitar un certificado de servidor de Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Para obtener información acerca de cómo instalar un autoservicio\-firmado de certificados, vea [IIS 7.0: Crear un\-firmado el certificado de servidor en IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
