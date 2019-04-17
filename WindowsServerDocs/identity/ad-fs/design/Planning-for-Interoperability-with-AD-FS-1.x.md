---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: Planear para la interoperabilidad con AD FS 1.x
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f287261ce6cb56e40385ef4de922045153819a23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>Planear para la interoperabilidad con AD FS 1.x

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de federación de Active Directory servicios de federación \(AD FS\) ejecutan Windows Server® 2012 pueden interactuar con ambas un AD FS 1.0 \ (que se instala con Windows Server 2003 R2\) servicios de federación de y una AD FS 1.1 \ (que se instala con Windows Server 2008 o Windows Server 2008 R2\) servicios de federación. Cualquiera de las combinaciones de interoperabilidad siguientes son compatibles:  
  
-   Cualquier 1 de AD FS. *x* servicios de federación puede enviar una reclamación que puede consumir un servicio de federación de AD FS en Windows Server 2012. Para obtener más información, consulta [lista de comprobación: configurar AD FS reclamaciones consumir desde AD FS 1.x](../../ad-fs/deployment/Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
-   Los servicios de federación de AD FS en Windows Server 2012 puede enviar un 1 de AD FS. *x*reclamación \-compatible que puede consumir un 1 de AD FS. *x* servicios de federación. Para obtener más información, consulta [lista de comprobación: configurar AD FS para enviar notificaciones a un AD FS 1.x servicios de federación de](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Los servicios de federación de AD FS en Windows Server 2012 puede enviar un 1 de AD FS. *x*reclamación \-compatible que puede usarse por uno o varios de los servidores Web que se ejecutan el 1 de AD FS. *x* claims\ cuenta agente de Web. Para obtener más información, consulta [lista de comprobación: configurar AD FS para enviar notificaciones a un AD FS 1.x notificaciones agente de Web](../../ad-fs/deployment/Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
> [!NOTE]  
> AD FS no admite o interactuar con el 1 de AD FS. *x* agente de Web de Windows NT basado en token.  
  
Un 1 de AD FS. *x*\-compatible reclamación es una notificación que se puede enviar por un servicio de federación de AD FS en Windows Server 2012 y comprender por una 1 de AD FS. *x* servicios de federación. Para que un 1 de AD FS. *x* servicios de federación de consumir las notificaciones que envía un servicio de federación de AD FS, debe enviar un tipo de notificación \(ID\) nombre identificador.  
  
## <a name="understanding-the-name-id-claim-type"></a>Comprender el tipo de notificación de Id. de nombre  
El tipo de notificación de Id. de nombre es el equivalente de la notificación de identidad escribe ese AD FS 1. *x* uses. Deben usarse siempre que quieras interoperar con AD FS 1. *x*. La notificación de nombre identificador escribe permite ya sea un 1 de AD FS. *x* servicios de federación o el 1 de AD FS. *x* agente de Web claims\ para consumir notificaciones que envía AD FS en Windows Server 2012, siempre que estas notificaciones se envían en uno de los formatos de Id. de nombre en la siguiente tabla.  
  
|Formato de nombre de Id.|URI correspondiente|  
|------------------|---------------------|  
|AD FS 1. *x* dirección de correo electrónico|http://schemas.xmlsoap.org/claims/EmailAddress|  
|AD FS 1. *x* UPN de correo electrónico|http://schemas.xmlsoap.org/claims/UPN|  
|Nombre común|http://schemas.xmlsoap.org/claims/CommonName|  
|Grupo|http://schemas.xmlsoap.org/claims/Group|  
  
Debe enviarse solo una notificación de Id. de nombre en el formato adecuado. Cuando este criterio se cumple, muchas de las demás reclamaciones podrán enviarse, suponiendo que se ajustan a las restricciones que se describen en la tabla.  
  
> [!NOTE]  
> Un 1 de AD FS. *x* servicios de federación puede interpretar los tipos de notificación entrante solo que comienzan con la \(URI\) identificador uniforme de recursos de http://schemas.xmlsoap.org/claims/.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
