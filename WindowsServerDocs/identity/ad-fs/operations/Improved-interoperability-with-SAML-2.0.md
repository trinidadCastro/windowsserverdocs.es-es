---
ms.assetid: 80b5335b-fa02-4944-900c-5fe4f5c6111d
title: Mejorar la interoperabilidad con SAML 2.0
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f55eaacec8ee0eb41e1980f1aa15c6256f8b979
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="improved-interoperability-with-saml-20"></a>Mejorar la interoperabilidad con SAML 2.0

>Se aplica a: Windows Server 2016

  
AD FS en Windows Server 2016 contiene compatibilidad de protocolo SAML adicional, incluida la capacidad de importar basado en metadatos que contiene varias entidades de confianza.  Esto te permite configurar AD FS para participar en confederaciones como federación InCommon y otras implementaciones de acuerdo con el estándar 2.0 eGov.   
  
La nueva funcionalidad se basa en los grupos de usuario de confianza o confianzas de proveedor de notificaciones. Cada grupo es un elemento EntitiesDescriptor (< md:EntitiesDescriptor >) contempladas en el eGov perfil 2.0, que contiene uno o varios elementos EntityDescriptor.  Los grupos tienen reglas comunes de autorización, y todas las demás propiedades pueden modificarse como objetos de confianza individuales.  
  
Una vez que se importan los grupos de confianza AD FS, AD FS actualiza automáticamente las relaciones de confianza como un grupo en el documento de metadatos.  
  
Habilitar estos escenarios es tan sencillo como con el nuevo commandlets de PowerShell que agregar y quitar AdfsClaimsProviderTrustsGroup y AdfsRelyingPartyTrustsGroup a objetos. Esto puede hacerse con una dirección URL de metadatos o un archivo, como se muestra en los ejemplos siguientes.  
  
Además, AD FS 2016 ofrece compatibilidad para el parámetro de ámbito tal como se describe en la especificación de SAML Core, sección 3.4.1.2. Este elemento permite confiar partes especificar uno o más proveedores de identidad para la autenticación de una solicitan.  
  
## <a name="examples"></a>Ejemplos  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataUrl "https://www.contosoconsortium.com/metadata/metadata.xml"   
```  
  
  
  
```  
Add-AdfsClaimsProviderTrustsGroup -MetadataFile "C:\metadata.xml"   
```  
  
## <a name="references"></a>Referencias  
  
Puede encontrar el perfil eGov 2.0 [aquí.](https://kantarainitiative.org/confluence/download/attachments/60817482/kantara-report-egov-saml2-profile-2.0.pdf?version=1&modificationDate=1345580916000&api=v2)  
  
La especificación SAML básica puede encontrarse [aquí.](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)   


