---
title: 'Solución de problemas de AD FS: certificados'
description: En este documento se describen los problemas típicos de los certificados.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: 7f209ba7a9afdb7a2522bcb167609ec823ddea8e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954211"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Solución de problemas de AD FS: certificados
AD FS requiere los certificados siguientes para funcionar correctamente.  Si alguno de estos no se ha configurado o configurado correctamente, pueden surgir problemas.

## <a name="required-certificates"></a>Certificados necesarios
AD FS requiere los siguientes certificados:



- **Confianza de Federación** : requiere que un certificado encadenado a una entidad de certificación (CA) raíz de Internet de confianza mutua esté presente en el almacén raíz de confianza del proveedor de notificaciones y de los servidores de Federación de usuario de confianza. se ha implementado un diseño de certificación cruzada en el que cada lado ha intercambiado su entidad de certificación raíz con su asociado o los certificados autofirmados que se han importado en cada lado cuando sea necesario.
- **Firma de tokens** : cada equipo servicio de Federación requiere un certificado de firma de tokens.  El certificado de firma de tokens del proveedor de notificaciones debe ser de confianza para el servidor de Federación de usuario de confianza. El certificado de firma de tokens de usuario de confianza debe ser de confianza para todas las aplicaciones que reciben tokens del servidor de Federación de RP.
- **Capa de sockets seguros (SSL)** : el certificado SSL para la servicio de Federación debe estar presente en un almacén de confianza del equipo de servidor proxy de Federación y tener una cadena válida a un almacén de entidades de certificación (CA) de confianza.
- **Lista de revocación de certificados (CRL)** : para cualquier certificado que tenga publicada una CRL, la CRL debe ser accesible para todos los clientes y servidores que necesitan tener acceso al certificado.

Si alguno de los anteriores no se ha configurado correctamente, AD FS no funcionará.

## <a name="common-things-to-check-with-certificates"></a>Aspectos comunes que se deben comprobar con los certificados
A continuación se muestra una lista de las cosas que pueden surgir y deben comprobarse al intentar resolver un problema de certificado.

- Asegúrese de que el certificado es de confianza.
    - Los clientes deben confiar en los certificados SSL
    - Los certificados de firma de tokens deben ser de confianza para los usuarios de confianza
- Compruebe la cadena de confianza: todos los certificados de la cadena deben ser válidos.
- Comprobar la fecha de expiración del certificado
- Comprobar accesibilidad de lista de revocación de certificados (CRL)
    - Asegúrese de que el campo CDP está rellenado
    - Examinar manualmente el CDP
- Asegúrese de que el certificado no se haya revocado.

## <a name="common-certificate-errors"></a>Errores comunes de certificados
En la tabla siguiente se muestra una lista de errores comunes y posibles causas.

|Evento|Causa|Resolución
|-----|-----|-----|
|Evento 249: no se encontró un certificado en el almacén de certificados. En escenarios de sustitución de certificados, esto puede producir un error si el Servicio de federación está firmando o descifrando con este certificado.|El certificado en cuestión no está presente en el almacén de certificados local o la cuenta de servicio no tiene permiso para la clave privada del certificado.|Asegúrese de que el certificado está instalado en el almacén de LocalMAchine\My en el servidor de AD FS. Asegúrese de que la cuenta de servicio de AD FS tenga acceso de lectura a la clave privada del certificado.|
|Evento 315: error al intentar crear la cadena de certificados para el certificado de firma de confianza del proveedor de notificaciones.|Se ha revocado el certificado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o aún no es válido.|Asegúrese de que el certificado es válido y que no se ha revocado.</br></br>Asegúrese de que la CRL sea accesible.|
|Evento 316: error al intentar crear la cadena de certificados para el certificado de firma de confianza del usuario de confianza.|Se ha revocado el certificado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o aún no es válido.|Asegúrese de que el certificado es válido y que no se ha revocado.</br></br>Asegúrese de que la CRL sea accesible.|
|Evento 317: error al intentar crear la cadena de certificados para el certificado de cifrado de la relación de confianza para usuario autenticado.|Se ha revocado el certificado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o aún no es válido.|Asegúrese de que el certificado es válido y que no se ha revocado.</br></br>Asegúrese de que la CRL sea accesible.|
|Evento 319: se produjo un error durante la creación de la cadena de certificados para el certificado de cliente.|Se ha revocado el certificado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o aún no es válido.|Asegúrese de que el certificado es válido y que no se ha revocado.</br></br>Asegúrese de que la CRL sea accesible.|
|Evento 360: se realizó una solicitud a un extremo de transporte de certificado, pero la solicitud no incluía un certificado de cliente.|La CA raíz que emitió el certificado de cliente no es de confianza.</br></br>El certificado de cliente ha expirado.</br></br>El certificado de cliente es autofirmado y no es de confianza.|Asegúrese de que la CA raíz que emitió el certificado de cliente está presente en el almacén raíz de confianza.</br></br>Asegúrese de que el certificado de cliente no haya expirado.</br></br>Si el certificado de cliente es autofirmado, asegúrese de que se ha agregado a la lista de certificados de confianza o reemplace el certificado autofirmado por un certificado de confianza.|
|Evento 374: se produjo un error al compilar la cadena de certificados para el certificado de cifrado de confianza del proveedor de notificaciones.|Se ha revocado el certificado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o aún no es válido.|Asegúrese de que el certificado es válido y que no se ha revocado.</br></br>Asegúrese de que la CRL sea accesible.|
|Evento 381: error al intentar crear la cadena de certificados para el certificado de configuración.|Uno de los certificados configurados para su uso en el servidor de AD FS ha expirado o se ha revocado.|Asegúrese de que todos los certificados configurados no se han revocado y no han expirado.|
|Evento 385: AD FS detectó que uno o varios certificados de la base de datos de configuración AD FS deben actualizarse manualmente.|Uno de los certificados configurados para su uso en el servidor de AD FS ha expirado o está llegando a su fecha de expiración.|Actualice el certificado expirado o pronto para expirar con un reemplazo. (Si usa certificados autofirmados y la sustitución automática de certificados está habilitada, este error puede omitirse, ya que se resolverá automáticamente).|
|Evento 387: AD FS detectó que uno o varios de los certificados especificados en el Servicio de federación no eran accesibles para la cuenta de servicio que usa el servicio de Windows de AD FS.|La cuenta de servicio de AD FS no tiene permisos de lectura para la clave privada de uno o más certificados configurados.|Asegúrese de que la cuenta de servicio de AD FS tiene permiso de lectura para la clave privada de todos los certificados configurados.|
|Evento 389: AD FS detectó que una o varias de sus confianzas requieren que sus certificados se actualicen manualmente porque han expirado o expirarán pronto.|Uno de los certificados de su socio configurado ha expirado o está a punto de expirar. Esto puede aplicarse a una relación de confianza para proveedor de notificaciones o a una relación de confianza para usuario autenticado.|Si ha creado manualmente esta confianza, actualice la configuración del certificado manualmente. Si usó metadatos de Federación para crear la confianza, el certificado se actualizará automáticamente en cuanto el socio actualice el certificado.|




## <a name="next-steps"></a>Pasos a seguir

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
