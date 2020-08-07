---
title: Consideraciones sobre las cuentas de usuario
description: Proporciona consideraciones de cuenta de usuario, nombre de usuario y contraseña para Multipoint Services
ms.topic: article
ms.assetid: e225900b-cee9-48c9-b21c-394dc5e72b78
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 10e58dcf21c9878ec632eee9160a0d3add4a6958
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951510"
---
# <a name="user-account-considerations"></a>Consideraciones sobre las cuentas de usuario
En este tema se describen problemas que usted, como usuario administrativo, debe tener en cuenta al crear y administrar cuentas de usuario. Las cuentas de usuario se administran en la pestaña usuarios de Multipoint Manager. Para más información, vea el tema [Administrar cuentas de usuario](Manage-User-Accounts.md).

## <a name="user-account-types"></a>Tipos de cuentas de usuario
Una cuenta de usuario es una colección de información que indica a multipoint Services a qué archivos y carpetas puede acceder un usuario, qué cambios pueden realizar en el sistema Multipoint Services y las preferencias de cada usuario, como el fondo de escritorio. Cada persona tiene acceso a su propia cuenta de usuario mediante un nombre de usuario y una contraseña únicos. Multipoint Services admite tres tipos de cuentas de usuario:

-   **Las cuentas de usuario administrativo** son para usuarios que usarán Multipoint Manager para usar y administrar el sistema Multipoint Services. Para más información, vea [Crear una cuenta de usuario administrativo](Create-an-Administrative-User-Account.md).

-   Las **cuentas de usuario estándar** son para aquellos usuarios que accederán con frecuencia a las estaciones, pero no administrarán el sistema. Por lo general, debería crear cuentas de usuario estándar para la mayoría de usuarios del sistema MultiPoint Services. Para más información, vea [Crear una cuenta de usuario estándar](Create-a-Standard-User-Account.md).

-   Las **cuentas de usuario de MultiPoint Dashboard** son para los usuarios que usan MultiPoint Dashboard para administrar sesiones de usuarios estándar y pueden iniciar sesión desde cualquier estación. Para más información, vea [Crear una cuenta de usuario de MultiPoint Dashboard](Create-a-MultiPoint-Dashboard-User-Account.md).

## <a name="user-name-and-password-considerations"></a>Consideraciones sobre el nombre de usuario y contraseña
Los usuarios administrativos pueden realizar tareas que afectan a los demás usuarios del sistema MultiPoint Services, como instalar software o cambiar la configuración de seguridad. Por este motivo, los usuarios administrativos deben tener nombres de usuario y contraseñas exclusivos que solo ellos conozcan.

Un aspecto importante relacionado con las cuentas de usuario es que a cada una se le asigna una biblioteca **Documentos** exclusiva del Explorador de Windows, que incluye la carpeta **Mis documentos**. Si los usuarios estándar del sistema MultiPoint Services van a almacenar documentos privados en su biblioteca **Documentos** en el Explorador de Windows, también deberían iniciar sesión en el sistema MultiPoint Services con un nombre de usuario y una contraseña únicos que solo ellos conozcan. Para más información sobre cómo almacenar documentos en el Explorador de Windows, vea el tema [Administrar archivos de usuario](Manage-User-Files.md).

> [!TIP]
> Para una mayor seguridad del sistema, todas las contraseñas de los usuarios deben ser contraseñas seguras. Una contraseña segura es aquella que no se puede adivinar o descifrar con facilidad, tiene al menos ocho caracteres de longitud, no contiene todo o parte del nombre de la cuenta del usuario y contiene al menos tres de las cuatro categorías de caracteres siguientes: caracteres en mayúsculas, caracteres en minúsculas, números y símbolos que se encuentran en un teclado (como!, @, #).

## <a name="see-also"></a>Consulte también
[Crear una cuenta](Create-an-Administrative-User-Account.md) 
 de usuario administrativo [Crear una cuenta](Create-a-Standard-User-Account.md) 
 de usuario estándar [Administrar archivos](Manage-User-Files.md) 
 de usuario [Administrar cuentas de usuario](Manage-User-Accounts.md)