---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configuración de AD FS para enviar notificaciones de expiración de contraseña
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 34a0544946da429f4e5b54a27b73c8bf139f5a8d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948567"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configuración de AD FS para enviar notificaciones de expiración de contraseña


Puede configurar Servicios de federación de Active Directory (AD FS) (AD FS) para enviar notificaciones de expiración de contraseñas a las relaciones de confianza para usuario autenticado (aplicaciones) que están protegidas por ADFS. La forma en que se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365 como usuario de confianza, las actualizaciones se han implementado en Exchange y Outlook para notificar a los usuarios federados de sus contraseñas que pronto están expiradas.

Para configurar AD FS para enviar notificaciones de expiración de contraseña a una relación de confianza para usuario autenticado, debe agregar las siguientes reglas de notificación a esta relación de confianza para usuario autenticado:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Las notificaciones de expiración de contraseña solo están disponibles para los tipos de autenticación de nombre de usuario y contraseña y Microsoft Passport for Work.  Si el usuario se autentica mediante la autenticación integrada de Windows y Passport no está configurado, las notificaciones no estarán disponibles y los usuarios no podrán ver las notificaciones de expiración de contraseñas.

> [!NOTE]
> Hay una ventana de 14 días, por lo que las notificaciones enviadas solo se rellenarán si la contraseña expira en 14 días.

## <a name="see-also"></a>Consulta también
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
