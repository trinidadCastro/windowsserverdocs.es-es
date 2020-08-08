---
title: Crear cuentas de usuario locales
description: Aprenda acerc thte tres tipos de cuentas de usuario en Multipoint Services
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: d13ea5e6d28e6d4574f888aa30f257ec1fe3fe04
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942484"
---
# <a name="create-local-user-accounts"></a>Crear cuentas de usuario locales
Se pueden crear tres niveles de cuentas de usuario locales en mediante Multipoint Manager: cuentas de usuario estándar; Usuarios de Multipoint Dashboard, que tienen derechos administrativos limitados; y cuentas de usuario administrativo completo.

Use el procedimiento siguiente para crear una cuenta de usuario local en un servidor MultiPoint Server. Si su entorno incluye varios servidores multipoint y desea que el usuario pueda iniciar sesión en cualquier estación de cualquier servidor, deberá crear una cuenta de usuario local en cada uno de los servidores. Esa configuración tiene algunas limitaciones. En un entorno de dominio, también puede permitir que los usuarios usen sus cuentas de dominio. Para obtener información general sobre las opciones, consulte [planeación de cuentas de usuario para el entorno de Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).

1.  Inicie sesión en el servidor como administrador y abra Multipoint Manager.

2.  Haga clic en la pestaña **usuarios** y, a continuación, haga clic en **Agregar cuenta de usuario**.

    Se abre el Asistente para agregar cuentas de usuario.

3.  Escriba un nombre de cuenta y una contraseña para la nueva cuenta de usuario y, a continuación, haga clic en **siguiente**.

4.  Seleccione el tipo de cuenta de usuario que desea crear:

    -   **Usuario estándar** : puede iniciar sesión en una estación y realizar tareas de usuario, pero no tiene acceso a multipoint Manager o al panel de MultiPoint Server, y no puede apagar el sistema.

    -   **Usuario de Multipoint Dashboard** : tiene derechos administrativos limitados. Un usuario del panel puede abrir el panel y realizar tareas como el registro de usuarios fuera del sistema o el apagado del equipo de MultiPoint Server, pero el usuario no tiene acceso a multipoint Manager.

    -   **Usuario administrativo** Tiene derechos administrativos completos en MultiPoint Server. Por ejemplo, un usuario administrativo puede ejecutar Multipoint Manager, agregar y eliminar usuarios, modificar la configuración del sistema y actualizar controladores.

5.  Haga clic en **siguiente**y, a continuación, haga clic en **Finalizar** para crear la cuenta de usuario.