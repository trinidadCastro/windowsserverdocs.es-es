---
description: 'Más información acerca de: certificados de Token-Signing'
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificados de firma de tokens
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b9fdfa0dddabd96623532d1b67dc2a189cde66d9
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946251"
---
# <a name="token-signing-certificates"></a>Certificados de firma de tokens

Los servidores de Federación requieren certificados de firma de tokens \- para evitar que los atacantes modifiquen o falsifican los tokens de seguridad en un intento de obtener acceso no autorizado a los recursos federados. El \/ emparejamiento de clave pública privada que se usa con \- los certificados de firma de tokens es el mecanismo de validación más importante de cualquier asociación federada, ya que estas claves comprueban que un token de seguridad fue emitido por un servidor de Federación de socio válido y que el token no se modificó durante el tránsito.

## <a name="token-signing-certificate-requirements"></a>Requisitos de certificado de firma de tokens \-
Un certificado de firma de tokens \- debe cumplir los siguientes requisitos para trabajar con AD FS:

-   Para que un certificado de firma de tokens \- firme correctamente un token de seguridad, el certificado de firma de tokens \- debe contener una clave privada.

-   La cuenta de servicio de AD FS debe tener acceso a la \- clave privada del certificado de firma de tokens en el almacén personal del equipo local. Esto se encarga del programa de instalación. También puede usar el complemento \- de administración de AD FS en para garantizar este acceso si posteriormente cambia el certificado de firma de tokens \- .

> [!NOTE]
> Se trata de una \( \) práctica recomendada de PKI de infraestructura de clave pública para no compartir la clave privada para varios propósitos. Por lo tanto, no use el certificado de comunicación de servicio que instaló en el servidor de Federación como certificado de firma de tokens \- .

## <a name="how-token-signing-certificates-are-used-across-partners"></a>Cómo se usan los certificados de firma de tokens \- en los asociados
Cada \- certificado de firma de tokens contiene claves privadas criptográficas y claves públicas que se usan para firmar digitalmente \( por medio de la clave privada \) un token de seguridad. Más adelante, una vez recibidos por un servidor de Federación de asociados, estas claves validan la autenticidad \( por medio de la clave pública \) del token de seguridad cifrado.

Dado que cada token de seguridad está firmado digitalmente por el asociado de cuenta, el asociado de recurso puede comprobar si el token de seguridad ha sido emitido realmente por el asociado de cuenta y si no se ha modificado. La parte de la clave pública del certificado de firma de tokens de un asociado comprueba las firmas digitales \- . Una vez comprobada la firma, el servidor de Federación de recursos genera su propio token de seguridad para su organización y firma el token de seguridad con su propio certificado de firma de tokens \- .

En el caso de los entornos de asociados de Federación, cuando \- una CA emite el certificado de firma de tokens, asegúrese de que:

1.  La revocación de certificados muestra las \( CRL \) del certificado a las que pueden acceder los usuarios de confianza y los servidores web que confían en el servidor de Federación.

2.  Los usuarios de confianza y los servidores web que confían en el servidor de Federación confían en el certificado de CA raíz.

El servidor Web del asociado de recurso utiliza la clave pública del certificado de firma de tokens \- para comprobar que el token de seguridad está firmado por el servidor de Federación de recursos. A continuación, el servidor web permite el acceso adecuado al cliente.

## <a name="deployment-considerations-for-token-signing-certificates"></a>Consideraciones de implementación para certificados de firma de tokens \-
Al implementar el primer servidor de Federación en una nueva instalación de AD FS, debe obtener un certificado de firma de tokens \- e instalarlo en el almacén de certificados personales del equipo local en ese servidor de Federación. Puede obtener un \- certificado de firma de tokens solicitando uno de una CA de empresa o una entidad de certificación pública, o bien mediante la creación de un \- certificado autofirmado.

Cuando se implementa una granja de AD FS, \- los certificados de firma de tokens se instalan de forma diferente, en función de cómo se cree la granja de servidores.

Hay dos opciones de granja de servidores que puede considerar al obtener \- los certificados de firma de tokens para la implementación:

-   Una clave privada de un certificado de firma de tokens \- se comparte entre todos los servidores de Federación de una granja.

    En un entorno de granja de servidores de Federación, se recomienda que todos los servidores de Federación compartan \( o reutilicen \) el mismo certificado de firma de tokens \- . Puede instalar un único \- certificado de firma de tokens desde una entidad de certificación en un servidor de Federación y, a continuación, exportar la clave privada, siempre que el certificado emitido se marque como exportable.

    Como se muestra en la siguiente ilustración, la clave privada de un certificado de firma de token único \- se puede compartir con todos los servidores de Federación de una granja. Esta opción, en comparación con la opción "único \- certificado de firma de tokens", reduce los costos si tiene previsto obtener un \- certificado de firma de tokens de una CA pública.

    ![La ilustración que muestra la clave privada de un solo certificado de firma de tokens \- puede compartirse en todos los servidores de Federación de una granja.](media/adfs2_fedserver_certstory_3.gif)

-   Hay un \- certificado de firma de tokens único para cada servidor de Federación de una granja.

    Cuando se usan varios certificados únicos en la granja, cada servidor de la granja firma los tokens con su propia clave privada única.

    Como se muestra en la siguiente ilustración, puede obtener un \- certificado de firma de tokens independiente para cada servidor de Federación de la granja. Esta opción es más costosa si tiene previsto obtener los \- certificados de firma de tokens de una CA pública.

    ![firma de tokens](media/adfs2_fedserver_certstory_4.gif)

Para obtener información acerca de cómo instalar un certificado al usar los servicios de Certificate Server de Microsoft como entidad de certificación empresarial, vea [iis 7,0: crear un certificado de servidor de dominio en iis 7,0](https://go.microsoft.com/fwlink/?LinkId=108548).

Para obtener información sobre la instalación de certificados desde una CA pública, vea [Solicitar un certificado de servidor de Internet (IIS 7)](https://go.microsoft.com/fwlink/?LinkId=108549).

Para obtener información acerca de cómo instalar un \- certificado autofirmado, vea [IIS 7,0: crear un \- certificado de servidor autofirmado en IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108271).

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
