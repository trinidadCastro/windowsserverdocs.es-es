---
title: 'Solución de problemas de AD FS: detección de bucles'
description: En este documento se describe cómo solucionar problemas de detección de bucles
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: e3d654f44ba75d9b647c0c1d9db7345c7ea75435
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953095"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>Solución de problemas de AD FS: detección de bucles

El bucle en AD FS se produce cuando un usuario de confianza rechaza continuamente un token de seguridad válido y redirige de nuevo a AD FS.

## <a name="loop-detection-cookie"></a>Cookie de detección de bucle
Para evitar que esto suceda, AD FS ha implementado lo que se denomina cookie de detección de bucles. De forma predeterminada, AD FS escribe una cookie en los clientes pasivos web denominados **MSISLoopDetectionCookie**. Esta cookie contiene un valor de marca de tiempo y un número de tokens emitidos.  Esto permite a AD FS realizar un seguimiento de la frecuencia y el número de veces que un cliente ha visitado el Servicio de federación dentro de un intervalo de tiempo específico.

Si un cliente pasivo visita el Servicio de federación para un token cinco (5) veces en 20 segundos, AD FS produce el siguiente error:

**MSIS7042: la misma sesión del explorador cliente realizó {0} solicitudes ' ' en los últimos ' {1} ' segundos. Póngase en contacto con el administrador para obtener más información.**

La entrada en bucles infinitos suele estar causada por una aplicación de usuario de confianza que no consume correctamente el token emitido por AD FS y la aplicación está enviando el cliente pasivo de nuevo a AD FS, repetidamente, para un nuevo token.  AD FS emite el cliente pasivo un nuevo token cada vez, siempre que no supere las 5 solicitudes en 20 segundos.

## <a name="adjusting-the-loop-detection-cookie"></a>Ajustar la cookie de detección de bucles
Puede usar PowerShell para cambiar el número de tokens emitidos y el valor TimeSpan.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
El valor mínimo de **LoopDetectionMaximumTokensIssuedInterval** es 1.

El valor mínimo de **LoopDetectionTimeIntervalInSeconds** es 5.

También puede deshabilitar la detección de bucles al realizar pruebas de rendimiento.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>No se recomienda deshabilitar permanentemente la detección de bucles, ya que esto impide que los usuarios entren en Estados de bucle infinito.


## <a name="next-steps"></a>Pasos a seguir

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)



