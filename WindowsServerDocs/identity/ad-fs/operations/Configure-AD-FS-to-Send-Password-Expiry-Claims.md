---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: "Configurar AD FS para enviar notificaciones de expiración de contraseña"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 386a5ac921ba609c371121b8657351667628951b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurar AD FS para enviar notificaciones de expiración de contraseña

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Puedes configurar servicios de federación de Active Directory (AD FS) para enviar notificaciones de expiración de contraseña para las confianzas de terceros de confianza (aplicaciones) que están protegidas por ADFS. ¿Cómo se usan estas notificaciones depende de la aplicación. Por ejemplo, con Office 365, como el usuario de confianza, las actualizaciones se han implementado en Exchange y Outlook para notificar a los usuarios federados de sus contraseñas pronto-a--expirar.

Para configurar AD FS para enviar contraseña expiración dice a una usuario de confianza confianza de terceros, debes agregar las siguientes reglas de notificación para esta confianza de terceros de confianza:

```
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
=> issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Reclamaciones de expiración de contraseña solo están disponibles para el nombre de usuario y contraseña y Microsoft Passport para tipos de autenticación de trabajo.  Si el usuario se autentica mediante la autenticación integrada de Windows y Passport no está configurado, las notificaciones no estará disponibles y los usuarios no verán notificaciones de expiración de contraseña.

> [!NOTE]
> Existe una ventana de 14 días para que las notificaciones envía se rellenará solo si la contraseña ha caducado en 14 días.

## <a name="see-also"></a>Consulta también
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)
