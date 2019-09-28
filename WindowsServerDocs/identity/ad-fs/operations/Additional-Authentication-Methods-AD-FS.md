---
title: Métodos de autenticación adicionales en AD FS 2019
description: En este documento se describen los nuevos métodos de autenticación en AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8a53a4cfca4f34459102b8edc8e6af82f36be70d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358368"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurar proveedores de autenticación de terceros como autenticación principal en AD FS 2019


Las organizaciones están experimentando ataques que intentan forzar la fuerza bruta, poner en peligro o bloquear las cuentas de usuario mediante el envío de solicitudes de autenticación basada en contraseñas.  Para ayudar a evitar que las organizaciones se pongan en peligro, AD FS ha incorporado capacidades como el bloqueo "inteligente" de la extranet y el bloqueo basado en direcciones IP.  

Sin embargo, estas mitigaciones son reactivas.  Para proporcionar una forma proactiva, para reducir la gravedad de estos ataques, AD FS tiene la capacidad de solicitar los factores que no son de contraseña antes de recopilar la contraseña.  

Por ejemplo, AD FS 2016 presentó Azure MFA como autenticación principal para que se puedan usar como primer factor los códigos OTP de la aplicación Authenticator.
Basándose en esto, con AD FS 2019 puede configurar proveedores de autenticación externos como factores de autenticación principales.

Hay dos escenarios clave que esto permite:

## <a name="scenario-1-protect-the-password"></a>Escenario 1: proteger la contraseña
Proteja el inicio de sesión basado en contraseña de ataques por fuerza bruta y bloqueos solicitando primero un factor adicional externo.  Solo si la autenticación externa se ha completado correctamente, el usuario verá un mensaje de solicitud de contraseña.  Esto elimina una forma cómoda de que los atacantes hayan estado intentando poner en peligro o deshabilitar cuentas.

Este escenario consta de dos componentes:
- Solicitud de Azure MFA o un factor de autenticación externo como autenticación principal
- Nombre de usuario y contraseña como autenticación adicional en AD FS

## <a name="scenario-2-password-free"></a>Escenario 2: sin contraseña.
Elimine las contraseñas por completo, pero complete una autenticación multifactor segura mediante métodos totalmente no basados en contraseña en AD FS
- Azure MFA con la aplicación Authenticator
- Windows 10 Hello para empresas
- Autenticación de certificado
- Proveedores de autenticación externos

## <a name="concepts"></a>Conceptos
Lo que significa realmente la **autenticación principal** es que es el método que se solicita primero al usuario, antes de factores adicionales.  Anteriormente, los únicos métodos principales disponibles en AD FS estaban integrados en métodos para Active Directory o Azure MFA, u otros almacenes de autenticación LDAP.  Los métodos externos se pueden configurar como autenticación "adicional", que tiene lugar después de que la autenticación principal se haya completado correctamente.

En AD FS 2019, la autenticación externa como capacidad principal significa que todos los proveedores de autenticación externos registrados en la granja de AD FS (con Register-AdfsAuthenticationProvider) están disponibles para la autenticación principal, así como "adicionales". autenticación. Se pueden habilitar de la misma manera que los proveedores integrados, como la autenticación de formularios y la autenticación de certificados, para el uso de la extranet o la intranet.

![autenticación](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Una vez que se haya habilitado un proveedor externo para la extranet, la intranet o ambos, estará disponible para que lo usen los usuarios.  Si hay más de un método habilitado, los usuarios verán una página de opciones y podrán elegir un método principal, de la misma manera que lo hacen para la autenticación adicional.

## <a name="pre-requisites"></a>Requisitos previos
Antes de configurar los proveedores de autenticación externos como principales, asegúrese de que tiene los siguientes requisitos previos en vigor:
- El nivel de comportamiento de la granja de AD FS (FBL) se ha elevado a ' 4 ' (este valor se convierte en AD FS 2019).
    - Este es el valor predeterminado de FBL para granjas de servidores de nuevas AD FS 2019
    - En el caso de las granjas de AD FS basadas en Windows Server 2012 R2 o 2016, el FBL se puede generar mediante el commandlet Invoke-AdfsFarmBehaviorLevelRaise de PowerShell.  Para obtener más información sobre la actualización de una granja de AD FS, consulte el artículo sobre la actualización de granja para granjas de servidores SQL o granjas WID 
    - Puede comprobar el valor de FBL mediante el cmdlet Get-AdfsFarmInformation
- La granja de AD FS 2019 está configurada para usar las nuevas páginas orientadas al usuario de 2019 ' paginadas '
    - Este es el comportamiento predeterminado de las granjas de servidores de New AD FS 2019
    - En el caso de las granjas de AD FS actualizadas desde Windows Server 2012 R2 o 2016, los flujos paginados se habilitan automáticamente cuando se habilita la autenticación externa como principal (la característica que se describe en este documento) tal y como se describe a continuación.

## <a name="enable-external-authentication-methods-as-primary"></a>Habilitar métodos de autenticación externos como principales
Una vez que haya comprobado los requisitos previos, hay dos maneras de configurar AD FS proveedores de autenticación adicionales como principal:

### <a name="using-powershell"></a>Con PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


El servicio de AD FS debe reiniciarse después de habilitar o deshabilitar la autenticación adicional como principal.

### <a name="using-the-ad-fs-management-console"></a>Usar la consola de administración de AD FS
En la consola de administración de AD FS, en**métodos de autenticación**de **servicio** -> , en métodos de **autenticación principales**, haga clic en Editar.

Haga clic en la casilla **permitir proveedores de autenticación adicionales como principal**.

El servicio de AD FS debe reiniciarse después de habilitar o deshabilitar la autenticación adicional como principal.

## <a name="enable-username-and-password-as-additional-authentication"></a>Habilitar el nombre de usuario y la contraseña como autenticación adicional
Para completar el escenario "proteger la contraseña", habilite el nombre de usuario y la contraseña como autenticación adicional mediante PowerShell o la consola de administración de AD FS.
### <a name="using-powershell"></a>Con PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>Usar la consola de administración de AD FS
En la consola de administración de AD FS, en**métodos de autenticación**de **servicio** -> , en métodos de **autenticación adicionales**, haga clic en **Editar** .

Haga clic en la casilla de **autenticación de formularios** para habilitar el nombre de usuario y la contraseña como autenticación adicional.
