---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Asignar los objetivos de implementación a un diseño de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866826"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Asignar los objetivos de implementación a un diseño de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando termines de revisar los servicios de federación de Active Directory existente \(AD FS\) objetivos de implementación y Determines qué objetivos están relacionados con la implementación, puede asignar esos objetivos a un diseño específico de AD FS. Para obtener más información acerca de AD FS predefinidos objetivos de implementación, consulte [identificar los objetivos de implementación de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Utilice la siguiente tabla para determinar qué diseño de AD FS que asigna a la combinación adecuada de AD FS objetivos de implementación para su organización. Esta tabla solo hace referencia a los dos diseños AD FS principales, como se describe en esta guía. Sin embargo, puede crear un híbrido o diseño personalizado de AD FS mediante cualquier combinación de los objetivos de implementación de AD FS para satisfacer las necesidades de su organización.  
  
|Objetivo de implementación de AD FS|[Diseño de SSO Web](Web-SSO-Design.md)|[Diseño de SSO Web federado](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Proporcionar acceso de los usuarios de Active Directory a los servicios y aplicaciones para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sí, en el asociado de cuenta|  
|[Proporcionar acceso de los usuarios de Active Directory a las aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sí, opcional en el asociado de cuenta|  
|[Proporcionar a los usuarios de otra organización acceso a los servicios y aplicaciones para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sí|Sí|  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

