---
title: Solución de problemas de AD FS-Fiddler-WS-Federation
description: En este documento se muestra un seguimiento detallado de un intercambio de WS-Federation con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a85d486dc30a36d575927ba243d46a403c3a5185
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520174"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>Solución de problemas de AD FS-Fiddler-WS-Federation

![Diagrama de Federación de AD FS y Windows Server](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Paso 1 y 2

Este es el principio de nuestro seguimiento.  En este marco se ve lo siguiente:

![Inicio del seguimiento de Fiddler](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

Solicitud:

- HTTP GET a nuestro usuario de confianza (http://sql1.contoso.com/SampApp)

Respuesta:

- La respuesta es un HTTP 302 (redirigir).  Los datos de transporte del encabezado de respuesta muestran adónde redirigirse a (https://sts.contoso.com/adfs/ls)
- La dirección URL de redireccionamiento contiene wa = wsignin 1,0, que nos indica que nuestra aplicación RP ha creado una solicitud de inicio de sesión de WS-Federation para nosotros y lo ha enviado al punto de conexión de/ADFS/LS/del AD FS.  Esto se conoce como enlace de redirección.

![Transporte de datos en el encabezado de respuesta](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Paso 3 y 4

![Seguimiento de Fiddler de continuación](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

Solicitud:

- HTTP GET a nuestro servidor de AD FS (STS. contoso. com)

Respuesta:

- La respuesta es un mensaje de solicitud de credenciales.  Esto indica que usamos Forms authnetication
- Al hacer clic en la vista previa de la respuesta, puede ver la solicitud de credenciales.

![Continuación del seguimiento de Fiddler](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Paso 5 y 6

![Pestaña WebView de la pantalla de solicitud de solicitud de credenciales](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

Solicitud:

- HTTP POST con el nombre de usuario y la contraseña.
- Presentamos nuestras credenciales.  Al examinar los datos sin procesar de la solicitud, podemos ver las credenciales

Respuesta:

- Se encuentra la respuesta y se crea y se devuelve la cookie cifrada MSIAuth.  Se utiliza para validar la aserción de SAML generada por el cliente.  Esto también se conoce como "cookie de autenticación" y solo estará presente cuando AD FS sea el IDP.

## <a name="step-7-and-8"></a>Paso 7 y 8

![Continuación del seguimiento de Fiddler](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

Solicitud:

- Ahora que hemos autenticado, hacemos otro HTTP GET en el servidor de AD FS y presento el token de autenticación.

Respuesta:

- La respuesta es un HTTP correcto, lo que significa que AD FS ha autenticado al usuario en función de las credenciales proporcionadas.
- Además, se establecen 3 cookies de nuevo en el cliente.
    - MSISAuthenticated contiene un valor de marca de tiempo codificada en base64 para cuando se autenticó el cliente.
    - MSISLoopDetectionCookie se usa en el mecanismo de detección de bucles infinitos AD FS para detener a los clientes que han terminado en un bucle de redirección infinito en el servidor de Federación. Los datos de la cookie son una marca de tiempo que está codificada en Base64.
    - MSISSignout se usa para realizar un seguimiento del IdP y de todos los RPs visitados para la sesión de SSO. Esta cookie se emplea cuando se invoca un cierre de sesión de WS-Federation. Puede ver el contenido de esta cookie con un descodificador de Base64.

## <a name="step-9-and-10"></a>Paso 9 y 10

![Continuación del seguimiento de Fiddler](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png)

Solicitud:

- HTTP POST

Respuesta:

- La respuesta es una encontrada

## <a name="step-11-and-12"></a>Paso 11 y 12

![Finalización del seguimiento de Fiddler](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png)

Solicitud:

- HTTP GET

Respuesta:

- La respuesta es correcta

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)