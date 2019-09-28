---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Mejora de la interoperabilidad con SAML 2.0
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d72636d77fe3240caab66dcab8657225d291bec6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407541"
---
# <a name="improved-interoperability-with-saml-20"></a>Mejora de la interoperabilidad con SAML 2.0



  
AD FS en Windows Server 2016 contiene compatibilidad con el protocolo SAML adicional, incluida la compatibilidad para importar confianzas basadas en metadatos que contienen varias entidades.  Esto le permite configurar AD FS para participar en confederaciones como la Federación infrecuente y otras implementaciones que se ajustan al estándar eGov 2,0.   
  
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


