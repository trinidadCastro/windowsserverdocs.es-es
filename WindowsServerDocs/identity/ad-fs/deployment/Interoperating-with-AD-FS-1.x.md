---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Interoperabilidad con AD FS 1.x
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 21f0ef0e3ad77d5ea58e5d3b82da66ce420e5d49
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972152"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperabilidad con AD FS 1.x

Para la interoperabilidad entre Servicios de federación de Active Directory (AD FS) \( AD FS \) en Windows Server &reg; 2012 y AD FS 1.* x*, realice una o varias de las siguientes tareas, en función de las necesidades de su organización:

-   Planee la interoperabilidad entre AD FS en Windows Server 2012 y versiones anteriores de AD FS y obtenga más información sobre el tipo de notificaciones de identificador de nombre. Para obtener más información, consulte [planificación de interoperabilidad con AD FS 1. x](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678040(v=ws.11)).

-   Si va a enviar notificaciones desde un AD FS Servicio de federación en Windows Server 2012 que se puede usar en una AD FS 1. *x* servicio de Federación, consulte [Checklist: Configuring AD FS to send Claims to a AD FS 1. x servicio de Federación](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).

-   Si va a enviar notificaciones desde un AD FS Servicio de federación en Windows Server 2012 que puede ser consumida por una aplicación hospedada por un servidor Web que ejecuta la AD FS 1. *x* \- agente Web compatible con notificaciones, consulte [lista de comprobación: configuración de AD FS para enviar notificaciones a un agente Web compatible con notificaciones de AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).

-   Si va a enviar notificaciones desde un AD FS 1. *x* servicio de Federación que va a consumir un AD FS servicio de Federación en Windows Server 2012, consulte [Checklist: Configuring AD FS para consumir notificaciones de AD FS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).

## <a name="differences-between-federation-service-settings"></a>Diferencias entre la configuración de Servicio de federación
Aunque la mayoría de las AD FS 1. la configuración de la Servicio de federación *x* funciona de forma similar a la Servicio de federación de AD FS en la configuración de Windows Server 2012, algunos nombres de configuración han cambiado. En la tabla siguiente se enumeran los nombres de los valores de un AD FS 1. *x* servicio de Federación y sus nombres equivalentes para una servicio de Federación AD FS en Windows Server 2012.

|Configuración de Servicio de federación de AD FS 1. x|AD FS equivalente Servicio de federación en el valor 2012 de Windows Server
|----------------------------------------|----------------------------------------------------------------------------------------------------------
|Asociado de cuenta|Confianza de proveedor de notificaciones
|Asociado de recurso|Confianza para usuario autenticado
|Application|Confianza para usuario autenticado
|Application Properties|Propiedades de la relación de confianza para usuario autenticado
|Dirección URL de la aplicación|Identificador de usuario de confianza y \- dirección URL de extremo pasivo de WS Federation
|URI Servicio de federación|Identificador del Servicio de federación
|Servicio de federación dirección URL del extremo|\-URL de extremo pasivo de Federación de WS

## <a name="see-also"></a>Consulte también
[Interoperabilidad de AD FS y AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)

