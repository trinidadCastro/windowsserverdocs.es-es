---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Asignar los objetivos de implementación a un diseño de AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4154f46bdb1a7b3a2c81eba6882a6a095a16ee45
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853058"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Asignar los objetivos de implementación a un diseño de AD FS


Después de terminar de revisar el Servicios de federación de Active Directory (AD FS) existente \(AD FS\) los objetivos de implementación y determinar qué objetivos están relacionados con la implementación, puede asignar esos objetivos a un diseño de AD FS específico. Para obtener más información sobre AD FS objetivos de implementación predefinidos, vea [identificar los objetivos de implementación de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Use la tabla siguiente para determinar qué diseño de AD FS se asigna a la combinación adecuada de los objetivos de implementación de AD FS para su organización. Esta tabla solo hace referencia a los dos diseños de AD FS principales, tal y como se describe en esta guía. Sin embargo, puede crear un diseño híbrido o personalizado de AD FS mediante el uso de cualquier combinación de los objetivos de implementación AD FS para satisfacer las necesidades de su organización.  
  
|AD FS objetivo de implementación|[Diseño de SSO web](Web-SSO-Design.md)|[Diseño de SSO web federado](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sí, en el asociado de cuenta|  
|[Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sí, opcional en el asociado de cuenta|  
|[Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sí|Sí|  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

