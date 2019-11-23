---
title: Solución de problemas de AD FS-inicio de sesión iniciado por IDP
description: En este documento se describe cómo solucionar problemas de la página de inicio de sesión de AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89ee45bd0387cf728bc126529169b6b1ca045d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366192"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>Solución de problemas de AD FS-inicio de sesión iniciado por IDP
La página de inicio de sesión de AD FS se puede usar para comprobar si la autenticación está funcionando o no.  Para ello, vaya a la página e inicie sesión.  Además, podemos usar la página de inicio de sesión para comprobar que se muestran todos los usuarios de confianza de SAML 2,0.

## <a name="enable-the-idp-initiated-sign-on-page"></a>Habilitación de la página de inicio de sesión iniciada por IDP
De forma predeterminada, AD FS en Windows 2016 no tiene habilitada la página de inicio de sesión.  Para habilitarlo, puede usar el comando de PowerShell Set-AdfsProperties.  Use el procedimiento siguiente para habilitar la página:

1.  Abrir Windows PowerShell
2.  Entrar: `Get-AdfsProperties` y presione Entrar
3.  Compruebe que **EnableIdpInitiatedSignonPage** está establecido en false ![false](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  En PowerShell, escriba: `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  No verá una confirmación, por lo que deberá escribir Get-AdfsProperties de nuevo y comprobar que **EnableIdpInitatedSignonPage** está establecido en true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Probar autenticación
Use el procedimiento siguiente para probar la autenticación de AD FS con la página de inicio de sesión iniciada por IDP.

1.  Abra un explorador Web y vaya a la página de inicio de sesión de IDP.  Ejemplo: https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Se le pedirá que inicie sesión.  Escriba sus credenciales.
](media/ad-fs-tshoot-initiatedsignon/idp5.png) de inicio de sesión ![
3.  Si esta operación se realizó correctamente, debe iniciar sesión.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Probar la autenticación con una experiencia de inicio de sesión sin problemas
Puede probar la experiencia de inicio de sesión sin problemas asegurándose de que la dirección URL de los servidores de AD FS se ha agregado a la zona Intranet local de las opciones de Internet.  Utilice el procedimiento siguiente:

1.  En un cliente de Windows 10, haga clic en Inicio y escriba opciones de Internet y seleccione Opciones de Internet.
2.   Haga clic en la pestaña seguridad, haga clic en Intranet local y, a continuación, haga clic en el botón sitios.
![](media/ad-fs-tshoot-initiatedsignon/idp8.png) sin problemas
1.  Haga clic en Avanzado.
2.  Escriba la dirección URL y haga clic en Agregar.  Haga clic en cerrar.
![agregar dirección URL](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Haga clic en Aceptar.  Haga clic en Aceptar.  Esto debe cerrar las opciones de Internet.
2.  Abra un explorador Web y vaya a la página de inicio de sesión de IDP.  Ejemplo: https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Haga clic en el botón iniciar sesión.  Debe iniciar sesión automáticamente y no se le pedirán las credenciales.
![](media/ad-fs-tshoot-initiatedsignon/idp6.png) sin problemas

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
