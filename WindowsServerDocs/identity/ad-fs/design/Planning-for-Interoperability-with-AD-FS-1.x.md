---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planificación para la interoperabilidad con AD FS 1.x
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e9f72bd83c90a804749329521a72e3232589c735
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407963"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planificación para la interoperabilidad con AD FS 1.x

Servicios de federación de Active Directory (AD FS) \(AD FS\) servidores de Federación que ejecutan Windows Server® 2012 pueden interoperar con AD FS 1,0 \(instalado con Windows Server 2003 R2\) servicio de Federación y un AD FS 1,1 \(instalado con Windows Server 2008 o Windows Server 2008 R2\) servicio de Federación. Se admite cualquiera de las siguientes combinaciones de interoperabilidad:  

-   Cualquier AD FS 1. *x* servicio de Federación puede enviar una demanda que un Servicio de federación de AD FS puede consumir en Windows Server 2012. Para obtener más información, consulte [Checklist: configuring AD FS para consumir notificaciones de AD FS 1. x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  

-   Cualquier AD FS Servicio de federación en Windows Server 2012 puede enviar un AD FS 1. *x*\-Claim compatible que puede consumir un AD FS 1. *x* servicio de Federación. Para obtener más información, consulte [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Federation Service](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  

-   Cualquier AD FS Servicio de federación en Windows Server 2012 puede enviar un AD FS 1. *x*\-notificaciones compatibles que pueden usar uno o varios servidores web que ejecuten el AD FS 1. el agente Web compatible con\-de notificaciones *x* . Para obtener más información, consulte [Checklist: Configuring AD FS to Send Claims to an AD FS 1.x Claims-Aware Web Agent](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  

> [!NOTE]  
> AD FS no admite ni interopera con el AD FS 1. *x* agente Web basado en tokens de Windows NT.  

Un AD FS 1. *x*\-Claim compatible es una demanda que se puede enviar mediante un AD FS servicio de Federación en Windows Server 2012 y que se entiende por un AD FS 1. *x* servicio de Federación. Para que un AD FS 1. *x* servicio de Federación puede consumir las notificaciones que envía un Servicio de federación de AD FS, se debe enviar un identificador de nombre \(identificador\) tipo de notificación.  

## <a name="understanding-the-name-id-claim-type"></a>Descripción del tipo de notificación de Id. de nombre  
El tipo de notificación de Id. de nombre es equivalente al tipo de notificación de identidad que usa AD FS 1.*x* . Debe usarse cuando quiera interactuar con AD FS 1.*x*. El tipo de notificaciones de identificador de nombre permite un AD FS 1. *x* servicio de Federación o el AD FS 1. *x* notifica\-agente Web compatible para consumir notificaciones que AD FS en Windows Server 2012 envía, siempre y cuando estas notificaciones se envíen en uno de los formatos de identificador de nombre de la tabla siguiente.  


|      Formato del Id. de nombre.       |               URI correspondiente                |
|---------------------------|------------------------------------------------|
| Dirección de correo electrónico de AD FS 1.*x* | http://schemas.xmlsoap.org/claims/EmailAddress |
|   UPN de correo electrónico de AD FS 1.*x*   |     http://schemas.xmlsoap.org/claims/UPN      |
|        Nombre común        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Grupo           |    http://schemas.xmlsoap.org/claims/Group     |

Solo se debe enviar una notificación de Id. de nombre con el formato adecuado. Cuando se cumple ese criterio, también se pueden enviar muchas otras notificaciones, suponiendo que se ajusten a las restricciones que se describen en la tabla.  

> [!NOTE]  
> Un AD FS 1. *x* servicio de Federación puede interpretar solo los tipos de notificaciones entrantes que comienzan por el identificador uniforme de recursos \(URI\) de http://schemas.xmlsoap.org/claims/.  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
