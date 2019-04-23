---
title: Solución de AD FS - detección de bucle
description: Este documento describe cómo solucionar problemas de detección de bucle
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc8eeb11e44da3b8f26b1ab94143c189bca9ed38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830916"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>Solución de AD FS - detección de bucle 
 
Bucle en AD FS se produce cuando un usuario de confianza rechaza un token de seguridad válido continuamente y redirige a AD FS.

## <a name="loop-detection-cookie"></a>Cookie de detección de bucle
Para evitar que esto suceda, AD FS ha implementado lo que se denomina una cookie de detección de bucle. De forma predeterminada, AD FS escribe una cookie a los clientes pasivos web denominados **MSISLoopDetectionCookie**. Esta cookie contiene un valor de marca de tiempo y un número de tokens emitidos valor.  Esto permite que AD FS para realizar un seguimiento de cómo y con qué frecuencia muchas veces que un cliente ha visitado el servicio de federación dentro de un intervalo de tiempo específico.

Si visita un cliente pasivo del servicio de federación para un token de cinco (5) veces dentro de 20 segundos, AD FS genera el error siguiente:

**MSIS7042: Ha realizado la misma sesión del explorador cliente '{0}"solicitudes en los últimos"{1}' segundos. Para obtener más información, póngase en contacto con su administrador.**

Escribir en bucles infinitos suele deberse a que se comportan mal para usuario autenticado de la aplicación que no está consumiendo correctamente el token emitido por AD FS y la aplicación está enviando al cliente pasivo a AD FS, varias veces, un nuevo token.  AD FS estará emitir el cliente pasivo un token nuevo cada vez, siempre que no superen 5 solicitudes dentro de 20 segundos. 

## <a name="adjusting-the-loop-detection-cookie"></a>Ajuste de la cookie de detección de bucle
Puede usar PowerShell para cambiar el número de tokens emitidos valor y el valor de intervalo de tiempo.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
El valor mínimo de **LoopDetectionMaximumTokensIssuedInterval** es 1.

El valor mínimo de **LoopDetectionTimeIntervalInSeconds** es 5.

También puede deshabilitar la detección de bucle cuando se realiza una prueba de rendimiento.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>Resulta que no se recomienda deshabilitar de forma permanente la detección de bucle como Esto impide que los usuarios escribir en los Estados de bucle infinito.


## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)



