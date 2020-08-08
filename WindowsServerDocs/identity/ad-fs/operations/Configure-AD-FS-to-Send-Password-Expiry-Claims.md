---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configuración de AD FS para enviar notificaciones de expiración de contraseña
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e53c33f1fef30570db46cf658bd1481343e846cd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962549"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configuración de AD FS para enviar notificaciones de expiración de contraseña


Puede configurar Servicios de federación de Active Directory (AD FS) (AD FS) para enviar notificaciones de expiración de contraseñas a las relaciones de confianza para usuario autenticado (aplicaciones) que están protegidas por ADFS. La forma en que se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365 como usuario de confianza, se han implementado actualizaciones en Exchange y Outlook para notificar a los usuarios federados sobre las contraseñas que pronto van a expirar.

Para configurar AD FS para enviar notificaciones de expiración de contraseña a una relación de confianza para usuario autenticado, debe agregar las siguientes reglas de notificación a esta relación de confianza para usuario autenticado:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Las notificaciones de expiración de contraseña solo están disponibles para los tipos de autenticación de nombre de usuario y contraseña y Microsoft Passport for Work.  Si el usuario se autentica mediante la autenticación integrada de Windows y Passport no está configurado, las notificaciones no estarán disponibles y los usuarios no podrán ver las notificaciones de expiración de contraseñas.

> [!NOTE]
> Hay una ventana de 14 días, por lo que las notificaciones enviadas solo se rellenarán si la contraseña expira en 14 días.

## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
