---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configuración de AD FS para enviar notificaciones de expiración de contraseña
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3be14b824038e9424b86c40bfd657dd988fa99e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189866"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configuración de AD FS para enviar notificaciones de expiración de contraseña


Puede configurar servicios de federación de Active Directory (AD FS) para enviar notificaciones de expiración de contraseña para el usuario autenticado (aplicaciones) que está protegida por AD FS. ¿Cómo se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365, como el usuario de confianza, se han implementado las actualizaciones para Exchange y Outlook para notificar a los usuarios federados de sus contraseñas pronto-a-haber-expirado.

Para configurar AD FS para enviar la contraseña de notificaciones de expiración para una entidad de confianza, debe agregar las siguientes reglas de notificación para esta relación de confianza:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Notificaciones de expiración de contraseña solo están disponibles para el nombre de usuario y contraseña y Microsoft Passport para tipos de autenticación del trabajo.  Si el usuario se autentica mediante la autenticación integrada de Windows y Passport no está configurada, las notificaciones no estará disponibles y los usuarios no verán notificaciones de expiración de contraseña.

> [!NOTE]
> Hay una ventana de 14 días para que las notificaciones enviadas se rellenará solamente si la contraseña caducará en 14 días.

## <a name="see-also"></a>Vea también
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)
