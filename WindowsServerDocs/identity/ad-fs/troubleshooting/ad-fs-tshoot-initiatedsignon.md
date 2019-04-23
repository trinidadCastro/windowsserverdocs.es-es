---
title: 'AD FS solución de problemas: iniciado por Idp inicio de sesión'
description: Este documento describe cómo solucionar problemas de la página Inicio de sesión de AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 61e9adc708e95a6ab4a82550280737b2f4bad0a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844946"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS solución de problemas: iniciado por Idp inicio de sesión
La página de inicio de sesión de AD FS se puede usar para probar si funciona la autenticación.  Para ello, vaya a la página e inicio de sesión.  Además, podemos usar la página de inicio de sesión para comprobar que se enumeran todas las entidades de usuario de confianza de SAML 2.0.

## <a name="enable-the-idp-intiated-sign-on-page"></a>Habilitar la página Inicio de sesión iniciado por Idp
De forma predeterminada, AD FS en Windows 2016 no tiene el inicio de sesión en la página habilitada.  Para permitir que se puede usar el comando de PowerShell Set-AdfsProperties.  Use el siguiente procedimiento para habilitar la página:

1.  Abra Windows PowerShell
2.  Escriba: `Get-AdfsProperties` y presione ENTRAR
3.  Compruebe que **EnableIdpInitiatedSignonPage** está establecida en false ![False](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  En PowerShell, escriba:  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  No verá una confirmación por lo tanto, vuelva a escribir Get-AdfsProperties y compruebe que **EnableIdpInitatedSignonPage** está establecido en true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Probar autenticación
Utilice el siguiente procedimiento para probar la autenticación de AD FS con la página Inicio de sesión Idp-Initiated.

1.  Abra un explorador web y vaya a la página Inicio de sesión de Idp.  Ejemplo:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Se le mostrará al inicio de sesión.  Escriba sus credenciales.
![Sign-on](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  Si esto se realizó correctamente debería iniciar sesión en.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Probar la autenticación mediante una experiencia de inicio de sesión sin problemas
Puede probar la experiencia de inicio de sesión integrado porque nos aseguramos de que la dirección URL de los servidores de AD FS se agregan a la zona intranet local de las opciones de internet.  Utilice el procedimiento siguiente:

1.  En un cliente de Windows 10, haga clic en Inicio y escriba las opciones de internet y seleccione opciones de internet.
2.   Haga clic en la ficha seguridad, haga clic en la intranet local y haga clic en el botón sitios.
![Seamless](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  Haga clic en Avanzado.
2.  Escriba la dirección url y haga clic en Agregar.  Haga clic en Cerrar.
![Agregar dirección url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Haga clic en Aceptar.  Haga clic en Aceptar.  Esto debería cerrar las opciones de internet.
2.  Abra un explorador web y vaya a la página Inicio de sesión de Idp.  Ejemplo:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Haga clic en el botón de inicio de sesión.  Debe iniciar sesión automáticamente y no se le pidan credenciales.
![Seamless](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)