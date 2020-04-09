---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Mejora de la interoperabilidad con SAML 2.0
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3abc3f09e5ae572800e5580d14a76ada6d62e320
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816368"
---
# <a name="improved-interoperability-with-saml-20"></a>Mejora de la interoperabilidad con SAML 2.0



  
AD FS en Windows Server 2016 contiene compatibilidad con el protocolo SAML adicional, incluida la compatibilidad para importar confianzas basadas en metadatos que contienen varias entidades.  Esto permite configurar AD FS para participar en confederaciones como la Federación de InCommon y otras implementaciones que se ajustan al estándar eGov 2.0.   
  
La nueva funcionalidad se basa en grupos de relaciones de confianza para proveedor de notificaciones o para usuario de confianza. Cada grupo es un elemento EntitiesDescriptor (< MD: EntitiesDescriptor >) tal y como se especifica en el perfil eGov 2,0, que contiene uno o varios elementos EntityDescriptor.  Los grupos tienen reglas de autorización comunes y todas las demás propiedades se pueden modificar como objetos de confianza individuales.  
  
Una vez que los grupos de confianza se importan en AD FS, AD FS actualiza automáticamente las confianzas como un grupo basado en el documento de metadatos.  
  
Habilitar estos escenarios es tan sencillo como usar los nuevos commandlets de PowerShell que agregan y quitan objetos AdfsClaimsProviderTrustsGroup y AdfsRelyingPartyTrustsGroup. Esto puede hacerse mediante una dirección URL de metadatos o un archivo, como se muestra en los ejemplos siguientes.  
  
Además, AD FS 2016 tiene compatibilidad con el parámetro de ámbito como se describe en la sección 3.4.1.2 de la especificación de SAML Core. Este elemento permite que los usuarios de confianza especifiquen uno o varios proveedores de identidades para una solicitud de autenticación.  
  
## <a name="examples"></a>Ejemplos  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Referencias  
  
El perfil eGov 2,0 se puede encontrar [aquí.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La especificación de SAML Core puede encontrarse [aquí.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


