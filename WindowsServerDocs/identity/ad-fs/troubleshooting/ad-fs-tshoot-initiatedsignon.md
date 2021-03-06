---
title: Solución de problemas de AD FS de Idp-Initiated de inicio de sesión
description: En este documento se describe cómo solucionar problemas de la página de inicio de sesión de AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.openlocfilehash: b6a3582ec8aa114362c3b4af15f7389ffa0bb52e
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781882"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>Solución de problemas de AD FS de Idp-Initiated de inicio de sesión
La página de inicio de sesión de AD FS se puede usar para comprobar si la autenticación está funcionando o no.  Para ello, vaya a la página e inicie sesión.  Además, podemos usar la página de inicio de sesión para comprobar que se muestran todos los usuarios de confianza de SAML 2,0.

## <a name="enable-the-idp-initiated-sign-on-page"></a>Habilitar la página de inicio de sesión de Idp-Initiated
De forma predeterminada, AD FS en Windows 2016 no tiene habilitada la página de inicio de sesión.  Para habilitarlo, puede usar el comando de PowerShell Set-AdfsProperties.  Use el procedimiento siguiente para habilitar la página:

1.  Abrir Windows PowerShell
2.  Escriba:  `Get-AdfsProperties` y presione Entrar.
3.  Compruebe que **EnableIdpInitiatedSignonPage** está establecido en false ![ false.](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  En PowerShell, escriba:  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  No verá una confirmación para que vuelva a escribir Get-AdfsProperties y compruebe que **EnableIdpInitatedSignonPage** está establecido en true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Probar autenticación
Utilice el siguiente procedimiento para probar la autenticación de AD FS con la página de inicio de sesión de Idp-Initiated.

1.  Abra un explorador Web y vaya a la página de inicio de sesión de IDP.  Ejemplo:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.aspx
2.  Se le pedirá que inicie sesión.  Escriba sus credenciales.
![Inicio de sesión](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  Si esta operación se realizó correctamente, debe iniciar sesión.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Probar la autenticación con una experiencia de inicio de sesión sin problemas

Puede probar la experiencia de inicio de sesión sin problemas asegurándose de que la dirección URL de los servidores de AD FS se ha agregado a la zona Intranet local de las opciones de Internet.  Utilice el siguiente procedimiento:

1. En un cliente de Windows 10, haga clic en Inicio y escriba opciones de Internet y seleccione Opciones de Internet.

1. Haga clic en la pestaña seguridad, haga clic en Intranet local y, a continuación, haga clic en el botón sitios.

    ![Captura de pantalla de la pestaña seguridad del cuadro de diálogo Propiedades de Internet que muestra la opción de Intranet local resaltada.](media/ad-fs-tshoot-initiatedsignon/idp8.png)

1. Haga clic en Avanzada.

1. Escriba la dirección URL y haga clic en Agregar.  Haga clic en Cerrar.

    ![Agregar dirección URL](media/ad-fs-tshoot-initiatedsignon/idp9.png)

1. Haga clic en Aceptar.  Haga clic en Aceptar.  Esto debe cerrar las opciones de Internet.

1. Abra un explorador Web y vaya a la página de inicio de sesión de IDP.  Ejemplo:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.aspx

1. Haga clic en el botón iniciar sesión.  Debe iniciar sesión automáticamente y no se le pedirán las credenciales.

    ![Captura de pantalla de la página de inicio de sesión en la que se muestra que el usuario no solicitó credenciales.](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
