---
title: Solución de AD FS - certificados
description: Este documento describe los problemas típicos de certificado.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 42fdce936bb4a6f25b456c4a96c7b99f650e6033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860586"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Solución de AD FS - certificados
AD FS requiere los siguientes certificados para poder funcionar correctamente.  Si cualquiera de ellos no se han de programa de instalación o configurado correctamente, a continuación, pueden surgir problemas.  

## <a name="required-certificates"></a>Certificados necesarios
AD FS requiere los siguientes certificados:



- **Confianza de federación** : Esto requiere que un certificado encadenado a una raíz de confianza mutua Internet entidad de certificación (CA) está presente en el almacén raíz de confianza del proveedor de notificaciones y de confianza en los servidores de federación, un se ha implementado el diseño de la certificación cruzada en el que cada lado tiene intercambiar su entidad de certificación raíz con su asociado o certificados que se han importado en cada lado donde corresponda autofirmados.
- **Firma de tokens** : el equipo de cada servicio de federación requiere un certificado de firma de tokens.  El certificado de firma de tokens del proveedor de notificaciones debe ser de confianza del servidor de federación de confianza. El certificado de firma de tokens de usuario de confianza entidad debe ser de confianza todas las aplicaciones que reciben tokens desde el servidor de federación de RP.
- **Secure Sockets Layer (SSL)** : el certificado SSL para el servicio de federación debe estar presente en un almacén de confianza en el equipo proxy de servidor de federación y tiene una cadena válida para un almacén de la entidad de certificación (CA) de confianza.
- **Lista de revocación (CRL) de certificados** : para cualquier certificado que tenga una CRL publicada, la CRL debe ser accesible para todos los clientes y servidores que necesitan tener acceso al certificado.

Si cualquiera de los pasos anteriores no están configurado correctamente, AD FS no funcionará.

## <a name="common-things-to-check-with-certificates"></a>Comprobaciones habituales con certificados
La siguiente es una lista de las cosas que pueden surgir y deben comprobarse al intentar resolver un problema de certificado.

- Asegúrese de que el certificado es de confianza
    - Deben ser de confianza para los clientes de certificados SSL
    - Certificados de firmas de tokens deben ser de confianza para los usuarios de confianza
- Compruebe la cadena de confianza - cada certificado en las necesidades de la cadena sea válido.
- Compruebe la fecha de expiración del certificado
- Comprobar la accesibilidad de la lista de revocación de certificados (CRL)
    - Asegúrese de que se rellena el campo CDP
    - Busque de forma manual el CDP
- Asegúrese de que no se revocó el certificado

## <a name="common-certificate-errors"></a>Errores comunes de certificado
En la tabla siguiente es una lista de errores comunes y las posibles causas.

|Evento|Causa|Resolución
|-----|-----|-----|
|Evento 249 - un certificado no se encontró en el almacén de certificados. En escenarios de sustitución del certificado, esto puede causar un error cuando el servicio de federación es firmar o descifrar utilizando este certificado.|El certificado en cuestión no está presente en el almacén de certificados local o la cuenta de servicio no tiene permiso para la clave privada del certificado.|Asegúrese de que el certificado está instalado en el almacén LocalMAchine\My en el servidor de AD FS. Asegúrese de que la cuenta de servicio de AD FS tenga acceso de lectura a la clave privada del certificado.|
|Evento 315 - se produjo un error al intentar compilar la cadena de certificados para el certificado de firma de confianza de proveedor de notificaciones.|El certificado se ha revocado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o todavía no es válido.|Asegúrese de que el certificado es válido y no se ha revocado.</br></br>Asegúrese de que la CRL es accesible.|
|Evento 316 - se produjo un error al intentar compilar la cadena de certificados para la relación de confianza certificado de firma.|El certificado se ha revocado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o todavía no es válido.|Asegúrese de que el certificado es válido y no se ha revocado.</br></br>Asegúrese de que la CRL es accesible.|
|Evento 317 - se produjo un error al intentar compilar la cadena de certificados para el certificado de cifrado de confianza para usuario autenticado de entidad.|El certificado se ha revocado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o todavía no es válido.|Asegúrese de que el certificado es válido y no se ha revocado.</br></br>Asegúrese de que la CRL es accesible.|
|Evento 319 - error se produjo mientras se está creando la cadena de certificados para el certificado de cliente.|El certificado se ha revocado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o todavía no es válido.|Asegúrese de que el certificado es válido y no se ha revocado.</br></br>Asegúrese de que la CRL es accesible.|
|Evento 360 - se realizó una solicitud a un punto de conexión de transporte de certificado, pero la solicitud no incluía un certificado de cliente.|La CA raíz que emitió el certificado de cliente no es de confianza.</br></br>Ha expirado el certificado de cliente.</br></br>El certificado de cliente es autofirmado y no es de confianza.|Asegúrese de que la CA raíz que emitió el certificado de cliente está presente en el almacén raíz de confianza.</br></br>Asegúrese de que el certificado de cliente no ha expirado.</br></br>Si el certificado de cliente es un certificado autofirmado, asegúrese de que se ha agregado a la lista de certificados de confianza o reemplazar el certificado autofirmado por un certificado de confianza.|
|Evento 374 - se produjo un error durante la compilación de la cadena de certificados para el proveedor de notificaciones de certificado de cifrado de confianza.|El certificado se ha revocado.</br></br>No se puede comprobar la cadena de certificados.</br></br>El certificado ha expirado o todavía no es válido.|Asegúrese de que el certificado es válido y no se ha revocado.</br></br>Asegúrese de que la CRL es accesible.|
|Evento 381 - se produjo un error durante un intento de crear la cadena de certificados para el certificado de configuración.|Uno de los certificados configurados para su uso en el servidor de AD FS ha expirado o se ha revocado.|Asegúrese de que todos los certificados configurados no se han revocado y no hayan expirado.|
|Evento 385 - AD FS ha detectado que uno o varios certificados en la base de datos de configuración de AD FS debe actualizarse manualmente.|Uno de los certificados configurados para su uso en el servidor de AD FS ha expirado o está a punto su fecha de expiración.|Actualice el certificado expirado o pronto-a-caducar con un reemplazo. (Si está utilizando certificados autofirmados y está habilitada la sustitución automática de certificados, este error se puede omitir tal como resolverá automáticamente.)|
|Evento 387 - AD FS ha detectado que uno o varios de los certificados que se especifican en el servicio de federación no eran accesibles para la cuenta de servicio que usa el servicio de Windows de AD FS.|La cuenta de servicio de AD FS no tiene permisos de lectura a la clave privada de uno o varios certificados configurados.|Asegúrese de que la cuenta de servicio de AD FS tiene el permiso a la clave privada de todos los certificados configurados de lectura.|
|Evento 389 - AD FS ha detectado que uno o varios de las confianzas requieren sus certificados que actualizarse manualmente porque han expirado o expirará pronto.|Uno de los certificados de su socio configurado ha expirado o está a punto de expirar. Esto se puede aplicar a una confianza de proveedor de notificaciones o a una relación de confianza para usuario autenticado.|Si ha creado manualmente esta relación de confianza, actualice manualmente la configuración de certificado. Si usa los metadatos de federación para crear la relación de confianza, el certificado se actualizará automáticamente tan pronto como el asociado de actualiza el certificado.|




## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
 