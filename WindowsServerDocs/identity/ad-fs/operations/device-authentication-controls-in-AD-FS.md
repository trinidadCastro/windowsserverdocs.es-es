---
title: Controles de autenticación de dispositivo en AD FS
description: Este documento describe cómo habilitar la autenticación de dispositivo en AD FS para Windows Server 2016 y 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822826"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticación de dispositivo en AD FS
El siguiente documento muestra cómo habilitar los controles de autenticación de dispositivo en Windows Server 2016 y 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticación de dispositivo en AD FS 2012 R2
Originalmente en AD FS 2012 R2 se ha producido una propiedad de autenticación global denominada `DeviceAuthenticationEnabled` que la autenticación de dispositivo controlado.

Para configurar la opción, el `Set-AdfsGlobalAuthenticationPolicy` cmdlet se usa como se muestra a continuación:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para deshabilitar la autenticación de dispositivo, se usó el mismo cmdlet para establecer el valor en $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticación de dispositivo en AD FS 2016
El único tipo de autenticación de dispositivo compatibles en 2012 R2 era clientTLS.  En AD FS 2016, además de clientTLS hay dos nuevos tipos de autenticación de dispositivos para la autenticación de dispositivos modernos.  Estos son:
- PKeyAuth
- PRT

Para controlar el comportamiento nuevo, el `DeviceAuthenticationEnabled` propiedad se utiliza en combinación con una nueva propiedad denominada `DeviceAuthenticationMethod`.  

El método de autenticación de dispositivo determina el tipo de autenticación de dispositivos que se hará: PRT, PKeyAuth, clientTLS o alguna combinación.
Tiene los siguientes valores:
 - SignedToken: PRT solo
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS 
 - Todo: Todos los anteriores

Como puede ver, PRT forma parte de todos los métodos de autenticación de dispositivo, lo que en efecto el método predeterminado que siempre está habilitado cuando `DeviceAuthenticationEnabled` está establecido en `$true`.

Por ejemplo: Para configurar los métodos, use el cmdlet DeviceAuthenticationEnabled como anteriormente, junto con la nueva propiedad:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> Al habilitar la autenticación de dispositivo (configuración `DeviceAuthenticationEnabled` a `$true`) significa que el `DeviceAuthenticationMethod` se establece implícitamente en `SignedToken`, que equivale a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>El método de autenticación de dispositivo predeterminado es `SignedToken`.  Otros valores son **PKeyAuth, *** ClientTLS,** y **todas**.

Los significados de los `DeviceAuthenticationMethod` valores han cambiado ligeramente desde el lanzamiento de AD FS 2016.  Consulte la tabla siguiente para el significado de cada valor, según el nivel de actualización:


|Versión de AD FS|Valor DeviceAuthenticationMethod|Medio|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Todos|PRT + PkeyAuth + clientTLS|
|2016 RTM + arriba hasta la fecha con Windows Update|SignedToken (es decir, modificado)|PRT (solo)|
||PkeyAuth (nuevo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Todos|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Vea también
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
