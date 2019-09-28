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
ms.openlocfilehash: d263f48aadff7c77cba44a2328d472ebbe5dfbbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407210"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>Solución de problemas de AD FS-Fiddler-WS-Federation
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Paso 1 y 2
Este es el principio de nuestro seguimiento.  En este marco se ve lo siguiente: ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

Solicite

- HTTP GET a nuestro usuario de confianza (http://sql1.contoso.com/SampApp)

Ante

- La respuesta es un HTTP 302 (redirigir).  Los datos de transporte del encabezado de respuesta muestran adónde redirigirse a (https://sts.contoso.com/adfs/ls)
- La dirección URL de redireccionamiento contiene wa = wsignin 1,0, que nos indica que nuestra aplicación RP ha creado una solicitud de inicio de sesión de WS-Federation para nosotros y lo ha enviado al punto de conexión de/ADFS/LS/del AD FS.  Esto se conoce como enlace de redirección.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Paso 3 y 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

Solicite

- HTTP GET a nuestro servidor de AD FS (STS. contoso. com)

Ante

- La respuesta es un mensaje de solicitud de credenciales.  Esto indica que usamos Forms authnetication
- Al hacer clic en la vista previa de la respuesta, puede ver la solicitud de credenciales.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Paso 5 y 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

Solicite

- HTTP POST con el nombre de usuario y la contraseña.  
- Presentamos nuestras credenciales.  Al examinar los datos sin procesar de la solicitud, podemos ver las credenciales

Ante

- Se encuentra la respuesta y se crea y se devuelve la cookie cifrada MSIAuth.  Se utiliza para validar la aserción de SAML generada por el cliente.  Esto también se conoce como "cookie de autenticación" y solo estará presente cuando AD FS sea el IDP.


## <a name="step-7-and-8"></a>Paso 7 y 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

Solicite

- Ahora que hemos autenticado, hacemos otro HTTP GET en el servidor de AD FS y presento el token de autenticación.

Ante

- La respuesta es un HTTP correcto, lo que significa que AD FS ha autenticado al usuario en función de las credenciales proporcionadas.
- Además, se establecen 3 cookies de nuevo en el cliente.
    - MSISAuthenticated contiene un valor de marca de tiempo codificada en base64 para cuando se autenticó el cliente.
    - MSISLoopDetectionCookie se usa en el mecanismo de detección de bucles infinitos AD FS para detener a los clientes que han terminado en un bucle de redirección infinito en el servidor de Federación. Los datos de la cookie son una marca de tiempo que está codificada en Base64.
    - MSISSignout se usa para realizar un seguimiento del IdP y de todos los RPs visitados para la sesión de SSO. Esta cookie se emplea cuando se invoca un cierre de sesión de WS-Federation. Puede ver el contenido de esta cookie con un descodificador de Base64.
    
## <a name="step-9-and-10"></a>Paso 9 y 10
Solicitud @no__t 0:

- HTTP POST

Ante

- La respuesta es una encontrada

## <a name="step-11-and-12"></a>Paso 11 y 12
Solicitud @no__t 0:

- HTTP GET

Ante

- La respuesta es correcta

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)