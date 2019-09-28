---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Implementación de servidores de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f2aaca5ffc846c41af82c276750c564db38b5020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359511"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperabilidad con AD FS 1.x

Para la interoperabilidad entre Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 en Windows Server® 2012 y AD FS 1. *x*, realice una o varias de las siguientes tareas, en función de las necesidades de su organización:  
  
-   Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre. Para obtener más información, consulte [planificación de interoperabilidad con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si va a enviar notificaciones desde un AD FS Servicio de federación en Windows Server 2012 que se puede usar en una AD FS 1. *x* servicio de Federación, consulte [Checklist: Configuración de AD FS para enviar notificaciones a un AD FS 1. x Servicio de federación @ no__t-0.  
  
-   Si va a enviar notificaciones desde un AD FS Servicio de federación en Windows Server 2012 que puede ser consumida por una aplicación hospedada por un servidor Web que ejecuta la AD FS 1. *x* Claims @ no__t-1aware web Agent, consulte [Checklist: Configuración de AD FS para enviar notificaciones a un agente Web compatible con notificaciones de AD FS 1. x @ no__t-0.  
  
-   Si va a enviar notificaciones desde un AD FS 1. *x* servicio de Federación que va a consumir un AD FS servicio de Federación en Windows Server 2012, vea [Checklist: Configuración de AD FS para consumir notificaciones de AD FS 1. x @ no__t-0.  
  
## <a name="differences-between-federation-service-settings"></a>Diferencias entre la configuración de Servicio de federación  
Aunque la mayoría de las AD FS 1. la configuración de la Servicio de federación *x* funciona de forma similar a la Servicio de federación de AD FS en la configuración de Windows Server 2012, algunos nombres de configuración han cambiado. En la tabla siguiente se enumeran los nombres de los valores de un AD FS 1. *x* servicio de Federación y sus nombres equivalentes para una servicio de Federación AD FS en Windows Server 2012.  
  
|Configuración de Servicio de federación de AD FS 1. x|AD FS equivalente Servicio de federación en el valor 2012 de Windows Server  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Asociado de cuenta|Confianza de proveedor de notificaciones  
|Asociado de recurso|Confianza para usuario autenticado 
|Application|Confianza para usuario autenticado  
|Propiedades de la aplicación|Propiedades de la relación de confianza para usuario autenticado  
|Dirección URL de la aplicación|Identificador de usuario de confianza y dirección URL de extremo pasivo de WS @ no__t-0Federation  
|URI Servicio de federación|Identificador del Servicio de federación  
|Servicio de federación dirección URL del extremo|WS @ no__t-0Federation: dirección URL de extremo pasivo  
  
## <a name="see-also"></a>Vea también  
[Interoperabilidad de AD FS y AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

