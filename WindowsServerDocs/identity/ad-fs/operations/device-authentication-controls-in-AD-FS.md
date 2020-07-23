---
title: Controles de autenticación de dispositivos en AD FS
description: En este documento se describe cómo habilitar la autenticación de dispositivos en AD FS para Windows Server 2016 y 2012 R2.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f9cc811c6a78e58ff3550343e89c19806b9914fb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966877"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticación de dispositivos en AD FS
En el siguiente documento se muestra cómo habilitar los controles de autenticación de dispositivos en Windows Server 2016 y 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticación de dispositivos en AD FS 2012 R2
Originalmente en AD FS 2012 R2 había una propiedad de autenticación global denominada `DeviceAuthenticationEnabled` que controlaba la autenticación de dispositivos.

Para configurar la opción, `Set-AdfsGlobalAuthenticationPolicy` se usó el cmdlet como se muestra a continuación:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para deshabilitar la autenticación de dispositivos, se usó el mismo cmdlet para establecer el valor en $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticación de dispositivos en AD FS 2016
El único tipo de autenticación de dispositivo admitido en 2012 R2 era clientTLS.  En AD FS 2016, además de clientTLS, hay dos nuevos tipos de autenticación de dispositivos para la autenticación de dispositivos modernos.  Son los siguientes:
- PKeyAuth
- PRT

Para controlar el nuevo comportamiento, la `DeviceAuthenticationEnabled` propiedad se usa en combinación con una nueva propiedad denominada `DeviceAuthenticationMethod` .  

El método de autenticación de dispositivos determina el tipo de autenticación de dispositivo que se realizará: PRT, PKeyAuth, clientTLS o alguna combinación.
Tiene los siguientes valores:
 - SignedToken: solo PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS
 - Todo: todo lo anterior

Como puede ver, PRT forma parte de todos los métodos de autenticación de dispositivos, lo que hace que esté en vigor el método predeterminado que siempre está habilitado cuando `DeviceAuthenticationEnabled` se establece en `$true` .

Ejemplo: para configurar los métodos, use el cmdlet DeviceAuthenticationEnabled como se indicó anteriormente, junto con la nueva propiedad:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> En ADFS 2019, `DeviceAuthenticationMethod` se puede usar con el `Set-AdfsRelyingPartyTrust` comando.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> La habilitación de la autenticación de dispositivos (si `DeviceAuthenticationEnabled` se establece en `$true` ) significa que `DeviceAuthenticationMethod` se establece implícitamente en `SignedToken` , lo que equivale a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> El método de autenticación de dispositivo predeterminado es `SignedToken` .  Otros valores son **PKeyAuth,**<strong>ClientTLS</strong> y **All**.

Los significados de los `DeviceAuthenticationMethod` valores han cambiado ligeramente desde que se liberó AD FS 2016.  Vea la tabla siguiente para ver el significado de cada valor, en función del nivel de actualización:


|Versión de AD FS|Valor DeviceAuthenticationMethod|Modo|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||All|PRT + PkeyAuth + clientTLS|
|2016 RTM + actualizado con Windows Update|SignedToken (significado cambiado)|PRT (solo)|
||PkeyAuth (nuevo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||All|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
