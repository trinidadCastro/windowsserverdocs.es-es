---
title: Crear una cuenta de usuario administrativo
description: Creación de una cuenta con privilegios administrativos en Multipoint Services
ms.topic: article
ms.assetid: 8ce4c5a9-3dec-412f-910b-54a252f8f209
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 451f090bb56d0fc9c4c801d128bdb5ae1df885ff
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971762"
---
# <a name="create-an-administrative-user-account"></a>Crear una cuenta de usuario administrativo
Cree *cuentas de usuario administrativo* para las personas que van a administrar el sistema MultiPoint Services. Para ver quién tiene acceso administrativo, en Multipoint Manager, haga clic en la pestaña **usuarios** . las cuentas de usuario administrativo se muestran en la columna **tipo de cuenta** como **Administrador**. *Los usuarios administrativos* tienen acceso a todas las tareas de Multipoint Manager que cambian la configuración del escritorio y del sistema, como:

-   Crear cuentas

-   Agregar y quitar programas

-   Administrar *escritorios* y hardware

-   Finalizar otras *sesiones de usuario*

Los usuarios administrativos pueden realizar tareas que afectan a los demás usuarios del sistema MultiPoint Services, como instalar software o cambiar la configuración de seguridad. Por este motivo, los usuarios administrativos deben tener nombres de usuario y contraseñas exclusivos que solo ellos conozcan.

Para más información sobre los aspectos que debería tener en cuenta el usuario administrativo al crear y administrar cuentas de usuario, vea el tema [Consideraciones sobre las cuentas de usuario](User-Account-Considerations.md).

> [!NOTE]
> Puede que le interese crear una *cuenta de usuario estándar* para usarla al realizar tareas en el sistema MultiPoint Services que no estén relacionadas con la administración del sistema MultiPoint Services. Después solo tendría que iniciar sesión en su cuenta de usuario administrativo cuando vaya a realizar tareas de administración del sistema.

#### <a name="to-create-an-administrative-user-account"></a>Para crear una cuenta de usuario administrativo

1.  En Multipoint Manager, haga clic en la pestaña **usuarios** .

2.  En **Tareas de usuario**, haga clic en **Agregar cuenta de usuario**. Se abrirá el asistente **Agregar cuenta de usuario**.

3.  En el campo **Cuenta de usuario**, escriba un nombre de inicio de sesión para el usuario. Por lo general, el nombre de usuario de inicio de sesión es el nombre y apellido escritos juntos sin espacios, o la inicial y apellido escritos juntos sin espacio.

4.  En el campo **nombre completo** , escriba el nombre del usuario en el formato que prefiera, como nombre dado, nombre completo o un alias.

5.  En el campo **Contraseña**, escriba una contraseña para el usuario. Solo usted y el usuario deben conocer la contraseña, y esta información debe almacenarse en una ubicación segura. Solo el usuario administrativo puede cambiar la contraseña.

6.  En el campo **Confirmar contraseña**, vuelva a escribir la contraseña y, después, haga clic en **Siguiente**.

7.  En la página de nivel de acceso, seleccione **Usuario administrativo** y haga clic en **Siguiente**.

8.  MultiPoint Services comprobará toda la información y mostrará un mensaje cuando la cuenta esté configurada. Cuando vea el texto **A new user account was successfully created** (Se ha creado correctamente una nueva cuenta de usuario), haga clic en **Finalizar**.
