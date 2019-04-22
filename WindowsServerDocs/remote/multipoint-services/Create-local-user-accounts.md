---
title: Crear cuentas de usuario locales
description: Obtenga información sobre acerc thte tres tipos de cuentas de usuario de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 33321932-4266-4961-9924-2cb4620bfcb4
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: e2e5a6c79e6dcd603d19ca868df1d11fce2bf746
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823676"
---
# <a name="create-local-user-accounts"></a>Crear cuentas de usuario locales
Tres niveles de las cuentas de usuario local se pueden crear en mediante el Administrador de MultiPoint: Cuentas de usuario estándares Usuarios de multiPoint Dashboard, que tienen derechos administrativos; limitados y las cuentas de usuario administrativo completas.  
  
Utilice el procedimiento siguiente para crear una cuenta de usuario local en un servidor MultiPoint. Si su entorno incluye varios servidores MultiPoint, y desea que el usuario sea capaz de iniciar sesión en cualquier estación en cualquier servidor, deberá crear una cuenta de usuario local en cada uno de los servidores. Que la instalación tiene algunas limitaciones. En un entorno de dominio, también puede permitir a los usuarios usar sus cuentas de dominio. Para obtener información general de las opciones, consulte [Plan de cuentas de usuario para el entorno de Windows MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md).  
   
1.  Inicie sesión en el servidor como administrador y abra MultiPoint Manager.  
  
2.  Haga clic en el **usuarios** pestaña y, a continuación, haga clic en **Agregar cuenta de usuario**.  
  
    Se abre el Asistente para agregar la cuenta de usuario.  
  
3.  Escriba un nombre de cuenta y una contraseña para la nueva cuenta de usuario y, a continuación, haga clic en **siguiente**.  
  
4.  Seleccione el tipo de cuenta de usuario que desea crear:  
  
    -   **Usuario estándar** : puede iniciar sesión en una estación y realizar tareas de usuario, pero no tiene acceso a MultiPoint Manager o el panel de MultiPoint Server y no se puede apagar el sistema.  
  
    -   **Usuario de multiPoint Dashboard** -tiene derechos administrativos limitados. Un usuario de panel se puede abrir el panel y realizar tareas como el sistema de los usuarios de la sesión o apagar el equipo de MultiPoint Server, pero el usuario no tiene acceso a MultiPoint Manager.  
  
    -   **Usuario administrativo** tiene derechos administrativos completos en MultiPoint Server. Por ejemplo, un usuario administrativo puede ejecutar el Administrador de MultiPoint, agregar y eliminar usuarios, modificar la configuración del sistema y actualizar los controladores.  
  
5.  Haga clic en **siguiente**y, a continuación, haga clic en **finalizar** para crear la cuenta de usuario.