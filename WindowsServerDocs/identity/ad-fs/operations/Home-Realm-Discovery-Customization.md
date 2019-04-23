---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Personalización de detección del dominio de inicio
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1198d8b76f2ecdad728e2de6ce7a5c0d053f779f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868936"
---
# <a name="home-realm-discovery-customization"></a>Personalización de detección del dominio de inicio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Cuando el cliente de AD FS primero solicita un recurso, el servidor de federación de recursos no tiene información sobre el dominio Kerberos del cliente. El servidor de federación de recursos responde al cliente de AD FS con un **detección de cliente del dominio** página, donde el usuario selecciona el dominio de inicio de una lista. Los valores de la lista se rellenar a partir de la propiedad del nombre para mostrar de las relaciones de confianza para proveedor de notificaciones. Use los siguientes cmdlets de Windows PowerShell para modificar y personalizar la experiencia de AD FS Home Realm Discovery.  
  
![dominio de inicio](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Ten en cuenta que el nombre del proveedor de notificaciones de Active Directory local que se muestra es el nombre para mostrar del servicio de federación.  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configuración del proveedor de identidades para utilizar ciertos sufijos de correo electrónico  
Cada organización se puede federar con varios proveedores de notificaciones. AD FS proporciona ahora la en\-cuadro capacidad para los administradores de la lista de sufijos, por ejemplo, @us.contoso.com, @eu.contoso.com, que son admitidos por un proveedor de notificaciones y habilitarlos para el sufijo\-detección basada en. Con esta configuración, los usuarios finales pueden escribir en su cuenta profesional y AD FS selecciona automáticamente el proveedor de notificaciones correspondiente.  
  
Para configurar un proveedor de identidades \(IDP\), tales como `fabrikam`, para utilizar ciertos sufijos de correo electrónico, utilice el cmdlet de Windows PowerShell y la sintaxis siguiente.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> Al federarse entre dos servidores de AD FS, establecer propiedad PromptLoginFederation en la confianza de proveedor de notificaciones para ForwardPromptAndHintsOverWsFederation.  Esto es para que AD FS reenviará el login_hint y el parámetro de símbolo del sistema para el proveedor de Identidades.  Esto puede hacerse mediante la ejecución del siguiente cmdlet de PowerShell:
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configuración de una lista de proveedores de identidades por usuarios de confianza  
En algunos escenarios, es posible que una organización quiera que los usuarios finales vean solamente los proveedores de notificaciones específicos de una aplicación, de manera que, en la página de detección del dominio de inicio, solo se muestre un subconjunto de los proveedores de notificaciones.  
  
Para configurar una lista de IDP por usuarios de confianza entidad \(RP\), use el siguiente cmdlet de Windows PowerShell y la sintaxis.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Omisión de la detección del dominio de inicio para la intranet  
La mayoría de las organizaciones solo ofrece su Active Directory local a los usuarios que acceden desde dentro del firewall. En esos casos, los administradores pueden configurar AD FS para que omita la detección del dominio de inicio para la intranet.  
  
Para omitir la HRD para la intranet, use el siguiente cmdlet de Windows PowerShell y la sintaxis.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Tenga en cuenta que si se ha configurado una lista de proveedores de identidad para un usuario de confianza, aunque la configuración anterior se ha habilitado y el usuario acceda desde la intranet, AD FS sigue mostrando la detección del dominio de inicio \(HRD\) página. Para omitir la HRD en este caso, tendrás que asegurarte de que se agregue también “Active Directory” a la lista de IDP del usuario de confianza.  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
