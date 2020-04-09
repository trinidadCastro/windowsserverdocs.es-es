---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Personalización de detección del dominio de inicio
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1667199e99545d3b29ba31512bd0c25718b93c4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816328"
---
# <a name="home-realm-discovery-customization"></a>Personalización de detección del dominio de inicio


Cuando el cliente de AD FS solicita primero un recurso, el servidor de Federación de recursos no tiene información sobre el dominio Kerberos del cliente. El servidor de Federación de recursos responde al cliente de AD FS con una página de **detección de dominio de cliente** , donde el usuario selecciona el dominio de inicio en una lista. Los valores de la lista se rellenar a partir de la propiedad del nombre para mostrar de las relaciones de confianza para proveedor de notificaciones. Use los siguientes cmdlets de Windows PowerShell para modificar y personalizar la experiencia de detección del dominio de inicio de AD FS.  
  
![dominio de inicio](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Ten en cuenta que el nombre del proveedor de notificaciones de Active Directory local que se muestra es el nombre para mostrar del servicio de federación.  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configuración del proveedor de identidades para utilizar ciertos sufijos de correo electrónico  
Cada organización se puede federar con varios proveedores de notificaciones. AD FS ahora proporciona la funcionalidad de Box en\-para que los administradores puedan enumerar los sufijos, por ejemplo, @us.contoso.com, @eu.contoso.com, que es compatible con un proveedor de notificaciones y habilitarlo para la detección basada en el sufijo\-. Con esta configuración, los usuarios finales pueden escribir su cuenta profesional y AD FS selecciona automáticamente el proveedor de notificaciones correspondiente.  
  
Para configurar un proveedor de identidades \(\)de IDP, como `fabrikam`, para usar determinados sufijos de correo electrónico, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> Al federar entre dos servidores AD FS, establezca la propiedad PromptLoginFederation en la relación de confianza para proveedor de notificaciones en ForwardPromptAndHintsOverWsFederation.  Esto es para que AD FS reenvíe el login_hint y solicite el parámetro al IDP.  Esto puede hacerse mediante la ejecución del siguiente cmdlet de PowerShell:
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configuración de una lista de proveedores de identidades por usuarios de confianza  
En algunos escenarios, es posible que una organización quiera que los usuarios finales vean solamente los proveedores de notificaciones específicos de una aplicación, de manera que, en la página de detección del dominio de inicio, solo se muestre un subconjunto de los proveedores de notificaciones.  
  
Para configurar una lista de IDP por usuario de confianza \(RP\), usa el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Omisión de la detección del dominio de inicio para la intranet  
La mayoría de las organizaciones solo ofrece su Active Directory local a los usuarios que acceden desde dentro del firewall. En esos casos, los administradores pueden configurar AD FS para omitir la detección del dominio de inicio para la intranet.  
  
Para omitir HRD para la intranet, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Tenga en cuenta que si se ha configurado una lista de proveedores de identidades para un usuario de confianza, aunque se haya habilitado la configuración anterior y el usuario acceda desde la intranet, AD FS seguirá mostrando la página\) de detección del dominio de inicio \(HRD. Para omitir la HRD en este caso, tendrás que asegurarte de que se agregue también “Active Directory” a la lista de IDP del usuario de confianza.  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
