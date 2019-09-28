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
ms.openlocfilehash: 87c011b18ad4a1d464072c1ea90b09a44e831378
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407362"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticación de dispositivos en AD FS
En el siguiente documento se muestra cómo habilitar los controles de autenticación de dispositivos en Windows Server 2016 y 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticación de dispositivos en AD FS 2012 R2
Originalmente en AD FS 2012 R2 había una propiedad de autenticación global denominada `DeviceAuthenticationEnabled` que controlaba la autenticación del dispositivo.

Para configurar el valor, se usó el cmdlet `Set-AdfsGlobalAuthenticationPolicy`, como se muestra a continuación:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para deshabilitar la autenticación de dispositivos, se usó el mismo cmdlet para establecer el valor en $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticación de dispositivos en AD FS 2016
El único tipo de autenticación de dispositivo admitido en 2012 R2 era clientTLS.  En AD FS 2016, además de clientTLS, hay dos nuevos tipos de autenticación de dispositivos para la autenticación de dispositivos modernos.  Estos son:
- PKeyAuth
- PRT

Para controlar el nuevo comportamiento, se usa la propiedad `DeviceAuthenticationEnabled` en combinación con una nueva propiedad llamada `DeviceAuthenticationMethod`.  

El método de autenticación de dispositivos determina el tipo de autenticación de dispositivo que se realizará: PRT, PKeyAuth, clientTLS o alguna combinación.
Tiene los siguientes valores:
 - SignedToken: Solo PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS PRT + clientTLS
 - Todo: Todos los anteriores

Como puede ver, PRT forma parte de todos los métodos de autenticación de dispositivos, lo que hace que esté en vigor el método predeterminado que siempre está habilitado cuando `DeviceAuthenticationEnabled` está establecido en `$true`.

Ejemplo: Para configurar los métodos, use el cmdlet DeviceAuthenticationEnabled como se indicó anteriormente, junto con la nueva propiedad:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> En ADFS 2019, se puede usar `DeviceAuthenticationMethod` con el comando `Set-AdfsRelyingPartyTrust`.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> La habilitación de la autenticación de dispositivos (si se establece `DeviceAuthenticationEnabled` en `$true`) significa que el @no__t 2 se establece implícitamente en `SignedToken`, que equivale a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> El método de autenticación de dispositivo predeterminado es `SignedToken`.  Otros valores son **PKeyAuth,** <strong>ClientTLS</strong> y **All**.

El significado de los valores de `DeviceAuthenticationMethod` ha cambiado ligeramente desde que se lanzó AD FS 2016.  Vea la tabla siguiente para ver el significado de cada valor, en función del nivel de actualización:


|Versión de AD FS|Valor DeviceAuthenticationMethod|Modo|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Todos|PRT + PkeyAuth + clientTLS|
|2016 RTM + actualizado con Windows Update|SignedToken (significado cambiado)|PRT (solo)|
||PkeyAuth (nuevo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Todos|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Vea también
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
