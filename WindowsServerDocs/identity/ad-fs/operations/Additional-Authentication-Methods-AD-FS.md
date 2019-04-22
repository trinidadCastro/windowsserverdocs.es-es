---
title: Métodos de autenticación adicionales de AD FS de 2019
description: Este documento describe los nuevos métodos de autenticación de AD FS de 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 68cd67dc14d3407985579a49e2f8603634fafdb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824436"
---
# <a name="configure-3rd-party-authenticaiton-providers-as-primary-authentication-in-ad-fs-2019"></a>Configurar proveedores de autenticación de terceros 3rd como autenticación principal en AD FS de 2019

>Se aplica a: Windows Server 2019

Las organizaciones experimentan los ataques que traten de fuerza bruta, poner en peligro o bloquear en caso contrario, las cuentas de usuario mediante el envío de solicitudes de autenticación basada en contraseña.  Para ayudar a proteger a las organizaciones de poner en peligro, AD FS ha introducido capacidades como bloqueo "inteligente" de la extranet y bloqueo de basados en direcciones IP.  

Sin embargo, estas mitigaciones son reactivas.  Para proporcionar una manera proactiva, para reducir la gravedad de estos ataques, AD FS tiene la capacidad de solicitar factores sin contraseña antes de recopilar la contraseña.  

Por ejemplo, AD FS 2016 presentó Azure MFA como autenticación principal para que los códigos de OTP de la aplicación Authenticator podrían utilizarse como el primer factor.
A partir de esto, con AD FS 2019 puede configurar proveedores de autenticación externos como factores de autenticación principal.

Hay dos escenarios clave, que esto permite:

## <a name="scenario-1-protect-the-password"></a>Escenario 1: proteger la contraseña
Proteger el inicio de sesión basado en contraseña frente a ataques de fuerza bruta y bloqueos de solicitando un factor adicional y externo en primer lugar.  Solo si se ha completado correctamente la autenticación externa el usuario, a continuación, ver un aviso de contraseña.  Esto elimina una manera cómoda de los atacantes se ha intentado poner en peligro o deshabilitar cuentas.

Este escenario consta de dos componentes:
- Preguntar para Azure MFA o un factor de autenticación externo como autenticación principal
- Nombre de usuario y contraseña como autenticación adicional en AD FS

## <a name="scenario-2-password-free"></a>Escenario 2: sin contraseña.
Eliminar contraseñas totalmente pero completar una fuerte, autenticación multifactor con la contraseña que no sea completamente según los métodos en AD FS
- Azure MFA con Authenticator
- 10 de Windows Hello para empresas
- Autenticación de certificado
- Proveedores de autenticación externos

## <a name="concepts"></a>Conceptos
¿Qué **autenticación principal** realmente significa es que es el método se solicita al usuario para el primero, antes de factores adicionales.  Los métodos sólo principales disponibles en AD FS se han compilado previamente en métodos para otro tipo de autenticación LDAP, Active Directory o Azure MFA almacenes.  Métodos externos se pueden configurar como autenticación "adicional", que tiene lugar tras la autenticación principal se haya completado correctamente.

En AD FS 2019, la autenticación externa como capacidad principal significa que todos los proveedores de autenticación externa registrados en la granja de AD FS (mediante Register-AdfsAuthenticationProvider) estén disponibles para la autenticación principal, así como "adicionales" autenticación. Se puede habilitar la misma manera que los proveedores integrados como autenticación de formularios y la autenticación de certificado, para intranet o extranet uso.

![autenticación](media/Additional-Authentication-Methods-AD-FS/auth1.png)

Una vez que un proveedor externo está habilitado para la extranet, intranet, o ambos, estará disponible para los usuarios utilicen.  Si más de un método está habilitado, los usuarios se vea una página de elección y poder elegir el método principal, al igual que para la autenticación adicional.

## <a name="pre-requisites"></a>Requisitos previos
Antes de configurar proveedores de autenticación externos como principal, asegúrese que de contar con los siguientes requisitos previos
- El nivel de comportamiento de granja de servidores de AD FS (FBL) se ha ampliado a "4" (este valor se convierte a AD FS 2019)
    - Este es el valor FBL predeterminado para nuevos granjas de servidores de AD FS de 2019
    - Para granjas de servidores de AD FS basados en Windows Server 2012 R2 o 2016, el FBL puede generarse mediante el cmdlet Invoke-AdfsFarmBehaviorLevelRaise de PowerShell.  Para obtener más información sobre cómo actualizar una granja de AD FS, consulte la granja de servidores de actualización de artículo para granjas de servidores SQL o granjas de servidores WID 
    - Puede comprobar el valor FBL mediante el cmdlet Get-AdfsFarmInformation
- La granja de servidores de AD FS 2019 está configurado para usar el nuevo usuario 'paginados' 2019 orientado a páginas
    - Este es el comportamiento predeterminado para granjas de servidores de AD FS 2019 nuevo
    - Para granjas de servidores de AD FS actualizadas desde Windows Server 2012 R2 o 2016, los flujos paginados se habilitan automáticamente cuando la autenticación externa como principal (la característica descrita en este documento) está habilitada como se describe a continuación.

## <a name="enable-external-authentication-methods-as-primary"></a>Habilitar los métodos de autenticación externos como principal
Una vez que haya comprobado los requisitos previos, hay dos formas de configurar los proveedores de autenticación adicionales de AD FS como principal:

### <a name="using-powershell"></a>Con PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


El servicio AD FS debe reiniciarse después de habilitar o deshabilitar la autenticación adicional como principal.

### <a name="using-the-ad-fs-management-console"></a>Mediante la consola de administración de AD FS
En la consola de administración de AD FS, en **servicio** -> **métodos de autenticación**, en **principal de métodos de autenticación**, haga clic en Editar

Haga clic en la casilla de verificación **permitir a los proveedores de autenticación adicionales como principal**.

El servicio AD FS debe reiniciarse después de habilitar o deshabilitar la autenticación adicional como principal.

## <a name="enable-username-and-password-as-additional-authentication"></a>Habilitar el nombre de usuario y contraseña como autenticación adicional
Para completar el escenario de "proteger la contraseña", habilite el nombre de usuario y contraseña como autenticación adicional mediante PowerShell o la consola de administración de AD FS
### <a name="using-powershell"></a>Con PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>Mediante la consola de administración de AD FS
En la consola de administración de AD FS, en **servicio** -> **métodos de autenticación**, en **métodos de autenticación adicionales**, haga clic en  **Editar**

Haga clic en la casilla de verificación **autenticación mediante formularios** para habilitar el nombre de usuario y contraseña como autenticación adicional.
