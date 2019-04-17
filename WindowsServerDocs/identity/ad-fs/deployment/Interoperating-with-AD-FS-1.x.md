---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: "Implementación de los servidores de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperar con AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para la interoperabilidad entre los servicios de federación de Active Directory \(AD FS\) en Windows Server® 2012 y AD FS 1. *x*, realice una o varias de las siguientes tareas, según las necesidades de la organización:  
  
-   Planear para la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtener que más información sobre el identificador de nombre de tipo de notificación. Para obtener más información, consulta [planeación para la interoperabilidad con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si va a enviar notificaciones desde un servicio de federación de AD FS en Windows Server 2012 que pueden consumir un 1 de AD FS. *x*, servicios de federación de consulta [lista de comprobación: configurar AD FS para enviar notificaciones a un AD FS 1.x servicios de federación de](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Si va a enviar notificaciones desde un servicio de federación de AD FS en Windows Server 2012 que puede usarse por una aplicación que está hospedada en un servidor Web que se ejecuta el 1 de AD FS. *x* claims\ cuenta agente de Web, consulta [lista de comprobación: configurar AD FS para enviar notificaciones a un AD FS 1.x notificaciones agente de Web](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Si va a enviar notificaciones de un 1 de AD FS. *x* servicios de federación de consumir un servicio de federación de AD FS en Windows Server 2012, consulta [lista de comprobación: configurar AD FS reclamaciones consumir desde AD FS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Diferencias entre la configuración de servicios de federación  
Aunque la mayoría de la AD FS 1. *x* trabajo de configuración de servicios de federación de forma similar, como el servicio de federación de AD FS en la configuración de Windows Server 2012, han cambiado algunos nombres de configuración. La siguiente tabla enumera los nombres de configuración de una 1 de AD FS. *x* servicios de federación y sus nombres equivalentes para un servicio de federación de AD FS en Windows Server 2012.  
  
|Configuración de servicios de federación de AD FS 1.x|Servicio de federación de AD FS equivalente en la configuración de Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partners de cuenta|Reclamaciones proveedor confianza  
|Partners de recursos|Usa la confianza de terceros 
|Aplicación|Usa la confianza de terceros  
|Propiedades de la aplicación|Usar las propiedades de confianza de terceros  
|Dirección URL de la aplicación|Usar el identificador de terceros y la dirección URL de extremo pasivo WS\ federación  
|Servicios de federación de URI|Identificador de servicios de federación  
|Dirección URL de extremo de servicio de federación|Dirección URL de extremo pasivo de federación de WS\  
  
## <a name="see-also"></a>Consulta también  
[AD FS y AD FS 1.x interoperabilidad](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

