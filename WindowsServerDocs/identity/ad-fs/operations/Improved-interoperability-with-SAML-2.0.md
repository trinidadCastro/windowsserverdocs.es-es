---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Mejora de la interoperabilidad con SAML 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4148614ba35ce29f567edb08b94e115d3f9152e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189105"
---
# <a name="improved-interoperability-with-saml-20"></a>Mejora de la interoperabilidad con SAML 2.0



  
AD FS en Windows Server 2016 contiene soporte de protocolo SAML adicionales, incluida la compatibilidad para la importación de las confianzas en función de metadatos que contiene varias entidades.  Esto le permite configurar AD FS para participar en confederaciones como federación InCommon y otras implementaciones que cumplen el estándar de 2.0 eGov.   
  
La nueva funcionalidad se basa en grupos de usuario de confianza o confianzas de proveedores de notificaciones. Cada grupo es un elemento de EntitiesDescriptor (< md:EntitiesDescriptor >) que especificó en el eGov perfil 2.0, que contiene uno o varios elementos de EntityDescriptor.  Los grupos tienen reglas de autorización comunes, y todas las demás propiedades se pueden modificar como objetos individuales de confianza.  
  
Una vez que se importan los grupos de confianza en AD FS, AD FS actualiza automáticamente las relaciones de confianza como un grupo basado en el documento de metadatos.  
  
Habilitar estos escenarios es tan sencillo como usar los cmdlets de PowerShell nueva que agregar y quitar AdfsClaimsProviderTrustsGroup y AdfsRelyingPartyTrustsGroup a objetos. Esto puede hacerse mediante una dirección URL de metadatos o un archivo, como se muestra en los ejemplos siguientes.  
  
Además, AD FS 2016 es compatible con el parámetro de ámbito como se describe en la especificación de SAML Core, sección 3.4.1.2. Este elemento permite confiar partes para especificar uno o más proveedores de identidades para la autenticación de una solicitud.  
  
## <a name="examples"></a>Ejemplos  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Referencias  
  
Puede encontrar el perfil eGov 2.0 [aquí.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
Puede encontrar la especificación de SAML Core [aquí.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


