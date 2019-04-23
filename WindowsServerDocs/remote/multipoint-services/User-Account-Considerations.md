---
title: Consideraciones sobre las cuentas de usuario
description: Proporciona la cuenta de usuario, nombre de usuario y contraseña consideraciones de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 00fb5e83921ba0b8ad86a6f75bdfd7bf16419b73
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850796"
---
# <a name="user-account-considerations"></a>Consideraciones sobre las cuentas de usuario
En este tema se describe problemas que, como usuario administrativo, debe considerar al crear y administra cuentas de usuario. Administrar cuentas de usuario en la pestaña usuarios de MultiPoint Manager. Para más información, vea el tema [Administrar cuentas de usuario](Manage-User-Accounts.md).  
  
## <a name="user-account-types"></a>Tipos de cuentas de usuario  
Una cuenta de usuario es una colección de información que indica a MultiPoint Services a qué archivos y carpetas puede acceder un usuario, qué cambios puede realizar en el sistema MultiPoint Services, así como las preferencias de cada usuario, como el fondo de escritorio. Cada persona tiene acceso a su propia cuenta de usuario mediante un nombre de usuario y una contraseña únicos. MultiPoint Services admite tres tipos de cuentas de usuario:  
  
-   **Cuentas de usuario administrativo** son para aquellos usuarios que van a usar MultiPoint Manager para usar y administrar el sistema MultiPoint Services. Para más información, vea [Crear una cuenta de usuario administrativo](Create-an-Administrative-User-Account.md).  
  
-   Las **cuentas de usuario estándar** son para aquellos usuarios que accederán con frecuencia a las estaciones, pero no administrarán el sistema. Por lo general, debería crear cuentas de usuario estándar para la mayoría de usuarios del sistema MultiPoint Services. Para más información, vea [Crear una cuenta de usuario estándar](Create-a-Standard-User-Account.md).  
  
-   Las **cuentas de usuario de MultiPoint Dashboard** son para los usuarios que usan MultiPoint Dashboard para administrar sesiones de usuarios estándar y pueden iniciar sesión desde cualquier estación. Para más información, vea [Crear una cuenta de usuario de MultiPoint Dashboard](Create-a-MultiPoint-Dashboard-User-Account.md).  
  
## <a name="user-name-and-password-considerations"></a>Consideraciones sobre el nombre de usuario y contraseña  
Los usuarios administrativos pueden realizar tareas que afectan a los demás usuarios del sistema MultiPoint Services, como instalar software o cambiar la configuración de seguridad. Por este motivo, los usuarios administrativos deben tener nombres de usuario y contraseñas exclusivos que solo ellos conozcan.  
  
Un aspecto importante relacionado con las cuentas de usuario es que a cada una se le asigna una biblioteca **Documentos** exclusiva del Explorador de Windows, que incluye la carpeta **Mis documentos**. Si los usuarios estándar del sistema MultiPoint Services van a almacenar documentos privados en su biblioteca **Documentos** en el Explorador de Windows, también deberían iniciar sesión en el sistema MultiPoint Services con un nombre de usuario y una contraseña únicos que solo ellos conozcan. Para más información sobre cómo almacenar documentos en el Explorador de Windows, vea el tema [Administrar archivos de usuario](Manage-User-Files.md).  
  
> [!TIP]  
> Para una mayor seguridad del sistema, todas las contraseñas de los usuarios deberían ser contraseñas seguras. Una contraseña segura es aquella que no se puede adivinar con facilidad o averiguada, es al menos ocho caracteres, no contiene todo o parte del nombre de la cuenta del usuario y contiene al menos tres de las siguientes cuatro categorías de caracteres: mayúsculas, minúsculas caracteres, números y símbolos del teclado (como!, @, #).  
  
## <a name="see-also"></a>Vea también  
[Crear una cuenta de usuario administrativo](Create-an-Administrative-User-Account.md)  
[Crear una cuenta de usuario estándar](Create-a-Standard-User-Account.md)  
[Administrar archivos de usuario](Manage-User-Files.md)
[administrar cuentas de usuario](Manage-User-Accounts.md)