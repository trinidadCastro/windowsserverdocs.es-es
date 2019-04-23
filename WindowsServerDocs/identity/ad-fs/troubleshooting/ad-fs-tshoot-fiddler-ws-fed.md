---
title: Solución de problemas - Fiddler - WS-Federation de AD FS
description: Este documento muestra un seguimiento detallado de un intercambio de WS-Federation con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be1c9f466ec13272d10f0fb9ca31cf326a1ec29a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846906"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>Solución de problemas - Fiddler - WS-Federation de AD FS
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Paso 1 y 2
Este es el principio de la traza.  En este marco se ve lo siguiente: ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

La solicitud:

- HTTP GET a nuestros usuarios de confianza (entidad http://sql1.contoso.com/SampApp)

Respuesta:

- La respuesta es HTTP 302 (Redireccionamiento).  Los datos de transporte en el encabezado de respuesta muestran dónde redirigir a)https://sts.contoso.com/adfs/ls)
- La dirección URL de redireccionamiento contiene wa = wsignin 1.0 que nos dice que nuestra aplicación de RP ha creado una solicitud de inicio de sesión de WS-Federation para nosotros y esto enviados a/ADFS/ls/punto de conexión de AD FS.  Esto se conoce como redirección de enlace.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Paso 3 y 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

La solicitud:

- HTTP GET para nuestro server(sts.contoso.com) de AD FS

Respuesta:

- La respuesta es un símbolo del sistema para las credenciales.  Esto indica que estamos usando formularios authnetication
- Al hacer clic en la vista Web de la respuesta puede ver las credenciales de símbolo del sistema.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Paso 5 y 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

La solicitud:

- HTTP POST con el nombre de usuario y la contraseña.  
- Presentamos nuestras credenciales.  Podemos ver las credenciales echando un vistazo a los datos sin procesar en la solicitud

Respuesta:

- La respuesta es encontrado y el MSIAuth se crea y devuelve una cookie cifrada.  Esto se utiliza para validar la aserción SAML producida por nuestros clientes.  Esto también es conocido como "cookie de autenticación" y solo estarán presente cuando AD FS es el proveedor de identidades.


## <a name="step-7-and-8"></a>Paso 7 y 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

La solicitud:

- Ahora que se han autenticado se realice otro método HTTP GET para el servidor de AD FS y presenta nuestro token de autenticación

Respuesta:

- La respuesta es HTTP OK lo que significa que AD FS ha autenticado al usuario según las credenciales proporcionadas
- Además, configuramos 3 cookies al cliente
    - MSISAuthenticated contiene un valor de marca de tiempo codificada en base64 para cuando el cliente se autenticó.
    - El mecanismo de detección de bucle infinito de AD FS a los clientes de palabras irrelevantes que finalizados en un bucle de redirección infinito para el servidor de federación usa MSISLoopDetectionCookie. Los datos de la cookie están una marca de tiempo que está codificado en base64.
    - MSISSignout se usa para realizar un seguimiento de los IdP y visitado todas las solicitudes por segundo para la sesión de inicio de sesión único. Esta cookie se utiliza cuando se invoca un cierre de sesión de WS-Federation. Puede ver el contenido de esta cookie con un descodificador de base64.
    
## <a name="step-9-and-10"></a>Paso 9 y 10
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) La solicitud:

- SOLICITUD HTTP POST

Respuesta:

- La respuesta es un encontrados

## <a name="step-11-and-12"></a>Paso 11 y 12
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) La solicitud:

- GET DE HTTP

Respuesta:

- La respuesta es correcto

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)