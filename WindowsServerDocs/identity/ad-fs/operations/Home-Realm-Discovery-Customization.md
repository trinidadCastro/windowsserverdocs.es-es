---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: "Personalización del detección de dominio de inicio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14b19e2774cf1eac2f5f23ea6737219611886b4c
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="home-realm-discovery-customization"></a>Personalización del detección de dominio de inicio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Cuando el cliente de AD FS primero solicita un recurso, el servidor de federación de recursos no tiene información sobre el ámbito del cliente. El servidor de federación de recursos responde al cliente de AD FS con un **detección del cliente territorio** página, donde el usuario selecciona el dominio de inicio de una lista. Se rellenan los valores de lista de la propiedad de nombre de la pantalla en el proveedor de reclamaciones confía. Usa los siguientes cmdlets de Windows PowerShell para modificar y personalizar la experiencia del detección de AD FS Home territorio.  
  
![Dominio de inicio](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Ten en cuenta que el nombre de proveedor de notificaciones que se muestra para Active Directory local es el nombre para mostrar federación servicio.  
  
## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurar el proveedor de identidad para usar determinados sufijos de correo electrónico  
Una organización puede federarse con varios proveedores de reclamaciones. AD FS ahora proporciona la capacidad de in\ cuadro para los administradores enumerar los sufijos, por ejemplo, @us.contoso.com, @eu.contoso.com, que es compatible con un proveedor de notificaciones y habilitarlo para la detección basada en suffix\. Con esta configuración, los usuarios finales escribir en su cuenta corporativa y AD FS selecciona automáticamente el proveedor de notificaciones correspondientes.  
  
Para configurar un proveedor de identidad \(IDP\), como `fabrikam`, para usar determinados sufijos de correo electrónico, usa el cmdlet de Windows PowerShell y la sintaxis siguiente.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
  
## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurar una lista de proveedores de identidad por confiar terceros  
Para algunos escenarios, las organizaciones sería mejor a los usuarios finales ver solo los proveedores de notificaciones que son específicos de una aplicación para que solo un subconjunto de proveedor de notificaciones se muestren en la página de detección de dominio de inicio.  
  
Para configurar una lista de IDP por confiar \(RP\) de terceros, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Pasar por alto Home territorio detección de la intranet  
La mayoría de las organizaciones solo admiten sus Active Directory local para cualquier usuario que accede desde dentro de su firewall. En esos casos, los administradores pueden configurar AD FS para omitir la detección de dominio de inicio de la intranet.  
  
Para omitir HRD para la intranet, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Por favor, ten en cuenta que si una lista de proveedores de identidad de un confiar parte se ha configurado, incluso aunque se ha habilitado la configuración anterior y los accesos de usuario de la intranet, AD FS aún muestra la página \(HRD\) de detección de dominio de inicio. Para omitir HRD en este caso, tienes que asegurarte de que "Active Directory" también se agregará a la lista de IDP para este usuario de confianza.  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
