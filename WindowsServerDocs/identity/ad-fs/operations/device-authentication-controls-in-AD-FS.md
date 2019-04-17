---
title: "Controles de dispositivo de autenticación de AD FS"
description: "Este documento describe cómo habilitar la autenticación de dispositivo en AD FS de Windows Server 2016 y 2012 R2"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de dispositivo de autenticación de AD FS
El documento siguiente muestra cómo habilitar los controles de autenticación de dispositivo en Windows Server 2016 y 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticación de dispositivo en AD FS 2012 R2
Originalmente en AD FS 2012 R2 había una propiedad global de autenticación llamada `DeviceAuthenticationEnabled`que la autenticación de dispositivo controlados.

Para configurar la configuración de la `Set-AdfsGlobalAuthenticationPolicy`cmdlet se usó como se muestra a continuación:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para deshabilitar la autenticación de dispositivo, se usó el mismo cmdlet para establecer el valor $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticación de dispositivo en AD FS de 2016
El único tipo de autenticación de dispositivo admitidos en 2012 R2 fue clientTLS.  En AD FS de 2016, además de clientTLS hay dos nuevos tipos de autenticación de dispositivo para la autenticación de dispositivos modernos.  Estos son:
- PKeyAuth
- IMP

Para controlar el comportamiento nuevo, la `DeviceAuthenticationEnabled`propiedad se usa en combinación con una nueva propiedad llamada `DeviceAuthenticationMethod`.  

El método de autenticación de dispositivo determina el tipo de autenticación de dispositivo que se hace: Imp, PKeyAuth, clientTLS, o alguna combinación.
Tiene los siguientes valores:
 - SignedToken: Imp solo
 - PKeyAuth: Imp + PKeyAuth
 - ClientTLS: Imp + clientTLS 
 - Todo: Todo lo anterior

Como puedes ver, Imp forma parte de todos los métodos de autenticación de dispositivo, lo que en realidad el método predeterminado que siempre está activado cuando `DeviceAuthenticationEnabled`se establece en `$true`.

Ejemplo: Para configurar los métodos, usa el cmdlet DeviceAuthenticationEnabled como anteriormente, junto con la nueva propiedad:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> Habilitar la autenticación de dispositivo (configuración `DeviceAuthenticationEnabled`a `$true`) significa que la `DeviceAuthenticationMethod`implícitamente se establece en `SignedToken`, que equivale a **Imp **.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>El método de autenticación de dispositivo predeterminado es `SignedToken`.  Otros valores son **PKeyAuth, *** ClientTLS,** y **todos los **.

El significado de los `DeviceAuthenticationMethod`valores han cambiado ligeramente desde el lanzamiento de AD FS de 2016.  Consulta la siguiente tabla para el significado de cada valor, según el nivel de actualización:


|Versión de AD FS|Valor DeviceAuthenticationMethod|Medio|
| ----- | ----- | ----- |
|RTM DE 2016|SignedToken|Imp + PkeyAuth|
||clientTLS|clientTLS|
||Todos los|Imp PkeyAuth + clientTLS|
|RTM de 2016 y actualizado con Windows Update|SignedToken (es decir, modificado)|Imp (solo)|
||PkeyAuth (novedad)|Imp + PkeyAuth|
||clientTLS|Imp + clientTLS|
||Todos los|Imp PkeyAuth + clientTLS|

## <a name="see-also"></a>Consulta también
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)
