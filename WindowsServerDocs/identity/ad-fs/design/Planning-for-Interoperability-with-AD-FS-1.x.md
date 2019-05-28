---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planificación para la interoperabilidad con AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7a1082b873f65a9f98b25425a392b2c62de8ca22
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191009"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planificación para la interoperabilidad con AD FS 1.x

Servicios de federación de Active Directory \(AD FS\) servidores de federación que ejecutan Windows Server® 2012 pueden interoperar con ambos AD FS 1.0 \(instalado con Windows Server 2003 R2\) AD FS y servicio de federación 1.1 \(instalado con Windows Server 2008 o Windows Server 2008 R2\) servicio de federación. Se admite cualquiera de las siguientes combinaciones de interoperabilidad:  
  
-   Cualquier AD FS 1. *x* servicio de federación puede enviar una notificación que puede consumir un servicio de federación de AD FS en Windows Server 2012. Para obtener más información, consulte [lista de comprobación: Configuración de AD FS para consumir notificaciones de AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   Cualquier servicio de federación de AD FS en Windows Server 2012 puede enviar AD FS 1. *x*\-notificación compatible que puede utilizarse en AD FS 1. *x* servicio de federación. Para obtener más información, consulte [lista de comprobación: Configuración de AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Cualquier servicio de federación de AD FS en Windows Server 2012 puede enviar AD FS 1. *x*\-notificación compatible que puede usarse en uno o más servidores Web que ejecuta AD FS 1. *x* notificaciones\-agente Web compatible con. Para obtener más información, consulte [lista de comprobación: Configuración de AD FS para enviar notificaciones a un agente Web para notificaciones de AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> AD FS no admite ni interactúa con AD FS 1. *x* agente Web basado en tokens de Windows NT.  
  
AD FS 1. *x*\-notificación compatible es una notificación que se puede enviar por un servicio de federación de AD FS en Windows Server 2012 y comprensibles para AD FS 1. *x* servicio de federación. Para que AD FS 1. *x* servicio de federación puede consumir las notificaciones que envía un servicio de federación de AD FS, un identificador de nombre \(ID\) se debe enviar un tipo de notificación.  
  
## <a name="understanding-the-nameid-claim-type"></a>Descripción del tipo de notificación de Id. de nombre  
El tipo de notificación de Id. de nombre es equivalente al tipo de notificación de identidad que usa AD FS 1.*x*. Debe usarse cuando quiera interactuar con AD FS 1.*x*. La notificación de identificador de nombre de tipo permite bien AD FS 1. *x* servicio de federación o AD FS 1. *x* notificaciones\-agente Web compatible con consuma las notificaciones que envía AD FS en Windows Server 2012, siempre y cuando dichas notificaciones se envíen en uno de los formatos de Id. de nombre en la tabla siguiente.  
  
|Formato del Id. de nombre.|URI correspondiente|  
|------------------|---------------------|  
|Dirección de correo electrónico de AD FS 1.*x*|http://schemas.xmlsoap.org/claims/EmailAddress|  
|UPN de correo electrónico de AD FS 1.*x*|http://schemas.xmlsoap.org/claims/UPN|  
|Nombre común|http://schemas.xmlsoap.org/claims/CommonName|  
|Agrupar|http://schemas.xmlsoap.org/claims/Group|  
  
Solo se debe enviar una notificación de Id. de nombre con el formato adecuado. Cuando se cumple ese criterio, también se pueden enviar muchas otras notificaciones, suponiendo que se ajusten a las restricciones que se describen en la tabla.  
  
> [!NOTE]  
> AD FS 1. *x* servicio de federación puede interpretar los tipos de notificación solo entrantes que comiencen con el identificador uniforme de recursos \(URI\) de http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
