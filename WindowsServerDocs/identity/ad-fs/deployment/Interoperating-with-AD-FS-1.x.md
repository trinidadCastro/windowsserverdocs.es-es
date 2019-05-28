---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Implementación de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eb9265513d5ca18da1150d3be6752d364b7cd1a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192083"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperabilidad con AD FS 1.x

Para la interoperabilidad entre servicios de federación de Active Directory \(AD FS\) en Windows Server® 2012 y AD FS 1. *x*, realice una o varias de las tareas siguientes, según las necesidades de su organización:  
  
-   Planear la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y aprenda que más sobre el identificador de nombre de tipo de notificación. Para obtener más información, consulte [planeamiento de la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si va a enviar notificaciones desde un servicio de federación de AD FS en Windows Server 2012 que pueden utilizarse en AD FS 1. *x* servicio de federación, consulte [lista de comprobación: Configuración de AD FS para enviar notificaciones a un servicio de federación de AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Si va a enviar notificaciones desde un servicio de federación de AD FS en Windows Server 2012 que pueden utilizarse en una aplicación que está hospedada en un servidor que ejecute AD FS 1. *x* notificaciones\-agente Web compatible con, consulte [lista de comprobación: Configuración de AD FS para enviar notificaciones a un agente Web para notificaciones de AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Si va a enviar notificaciones de AD FS 1. *x* servicio de federación para consumir un servicio de federación de AD FS en Windows Server 2012, consulte [lista de comprobación: Configuración de AD FS para consumir notificaciones de AD FS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Diferencias entre la configuración del servicio de federación  
Aunque la mayoría de AD FS 1. *x* funciona la configuración de servicio de federación de forma similar, como el servicio de federación de AD FS en la configuración de Windows Server 2012, que han cambiado algunos nombres de configuración. En la tabla siguiente se enumera los nombres de configuración de AD FS 1. *x* servicio de federación y sus nombres equivalente para un servicio de federación de AD FS en Windows Server 2012.  
  
|Configuración de servicio de federación de AD FS 1.x|Servicio de federación de AD FS equivalente en la configuración de Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Asociado de cuenta|Confianza de proveedor de notificaciones  
|Asociado de recurso|Confianza para usuario autenticado 
|Application|Confianza para usuario autenticado  
|Propiedades de la aplicación|Propiedades de confianza de entidades de usuario de confianza  
|Dirección URL de la aplicación|Identificador de entidad de usuario de confianza y WS\-dirección URL de extremo pasivo de federación  
|URI del servicio de federación|Identificador del Servicio de federación  
|Dirección URL del punto de conexión de servicio de federación|WS\-dirección URL del extremo pasivo de federación  
  
## <a name="see-also"></a>Vea también  
[AD FS y la interoperabilidad de AD FS 1.x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

